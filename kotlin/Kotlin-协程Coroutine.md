---
title: Kotlin-协程Coroutine
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 概念
协作程序，解决异步问题。协程在应用层完成，相比较线程，由操作系统调度，抢占cpu，协程不存在抢占。协程内的代码会挂起，等到执行结束再返回继续执行，协程外的代码正常执行，协程可以通过类似于同步的直观的同步代码，解决异步操作时复杂的逻辑问题，是一种轻量级的并发方案。

## suspend关键字
关键字修饰的函数，代表它可以被挂起。

## 同步下载图片1
工具类
``` kotlin
class BaseContinuation : Continuation<Unit> {
    override val context: CoroutineContext = EmptyCoroutineContext

    override fun resume(value: Unit) {
    }

    override fun resumeWithException(exception: Throwable) {
    }

}

// 单例下载类
object HttpService {
    val client = OkHttpClient()

    fun getLogo(url: String): Response {
        val request = Request.Builder().get().url(url).build()
        val respose = client.newCall(request).execute()
        return respose
    }
}

// Http异常类
data class HttpException(val code: Int): Exception()
``` 
协程代码
``` kotlin
// 开始协程函数
fun startMyCoroutine(block: suspend () -> Unit) {
    block.startCoroutine(BaseContinuation())
}

// ByteArray 想要返回的数据类型
suspend fun startDownloadPic(url: String) = suspendCoroutine<ByteArray> { continuation ->
    log("开始耗时操作")
    try {
	   // getLogo是一个自定义的基于OkHttp3的同步Get方法
        val response = HttpService.getLogo(url)
        if (response.isSuccessful) {
		// 获取到的数据交给continuation的resume方法返回，而整个函数的返回值就是suspendCoroutine的返回值byteArray
            response.body()?.bytes()?.let(continuation::resume)
        } else {
            continuation.resumeWithException(HttpException(response.code()))
        }
    } catch (e: Exception) {
        continuation.resumeWithException(e)
    }
}

Main.kt
    log("协程之前")
    startMyCoroutine {
        log("协程开始")
        val bytes = startDownloadPic(LOGO_URL)
        log("拿到图片")
		Bitmap bitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
        imageView.setImageBitmap(bitmap);
    }
    log("协程之后")
```
通过log可知，以上操作都在主线程中，没有实现异步。

## 异步下载图片2
工具类
```kotlin
// 线程池类
private val pool by lazy {
    Executors.newCachedThreadPool()
}

class AsyncTask(val block: () -> Unit) {
    fun execute() = pool.execute(block)
}
```

协程代码
不使用装饰类
```kotlin
suspend fun startDownloadPic(url: String, activity: Activity) = suspendCoroutine<ByteArray> { continuation ->
    log("异步任务开始前")
    AsyncTask {
        log("开始耗时操作")
        try {
            val response = HttpService.getLogo(url)
            if (response.isSuccessful) {
                response.body()?.bytes()?.let {
					// 异步结束之后切换回主线程继续后面的操作。
                    activity.runOnUiThread(Runnable {
                        continuation.resume(it)
                    })
                }
            } else {
                activity.runOnUiThread(Runnable {
                    continuation.resumeWithException(HttpException(response.code()))
                })
            }
        } catch (e: Exception) {
            continuation.resumeWithException(e)
        }
    }.execute()
}
```
关于切换回主线程以上是一种解决办法，也可以声明一个装饰类，在装饰类里面切换线程。（由于Android切换线程传参需要Activity，所以这里就不使用装饰类了）


``` kotlin
// CoroutineContext 运行上下文，资源持有，上下调度
// Continuation 运行控制类，负责结果和异常的返回
// ContinuationInterceptor协程控制拦截器，可用来处理协程调度

// 装饰类
class UiCotinuationWrapper<T>(val cotinuation: Continuation<T>) : Continuation<T> {
    override val context: CoroutineContext = cotinuation.context

    override fun resume(value: T) {
	    // 切换线程 cotinuation.resume(value)
    }

    override fun resumeWithException(exception: Throwable) {
		// 切换线程 cotinuation.resumeWithException(exception)
    }

}

// 自定义Continuation
class ContextContinuation(override val context: CoroutineContext = EmptyCoroutineContext) : Continuation<Unit> {
    override fun resume(value: Unit) {
    }

    override fun resumeWithException(exception: Throwable) {
    }

}

// 异步上下文 
// 将普通上下文转换成我们装饰类修饰的上下文，也就是结果会切换到主线程的上下文
class AsyncContext : AbstractCoroutineContextElement(ContinuationInterceptor), ContinuationInterceptor {
    override fun <T> interceptContinuation(continuation: Continuation<T>): Continuation<T> {
        // fold （初始值）{操作}
        return UiCotinuationWrapper(continuation.context.fold(continuation) {
            // continuation 经过每一步篡改后的对象
            // element 篡改continuation的对象（拦截器）
            continuation, element ->
            if (element != this && element is ContinuationInterceptor) {
                element.interceptContinuation(continuation)
            } else continuation
        })
    }

}

// 为保证可变参数的线程安全而声明的传参上下文
class DownloadContext(val url: String): AbstractCoroutineContextElement(Key){
    companion object Key: CoroutineContext.Key<DownloadContext>
} 
```

协程代码
```kotlin
fun startMyCoroutine(context: CoroutineContext = EmptyCoroutineContext, block: suspend () -> Unit) {
    block.startCoroutine(ContextContinuation(context + AsyncContext()))
}

suspend fun <T> startConsumeTime(block: CoroutineContext.() -> T) = suspendCoroutine<T> { continuation ->
    // 虽然传入continuation是普通的类型，但是之后我们耗时操作结束所使用的continuation是包装以后的
	// 至此，如果我们想使用其他功能性的协程，只要自定义一个上下文
    AsyncTask {
        try {
           continuation.resume(block(continuation.context))
        } catch (e: Exception) {
            continuation.resumeWithException(e)
        }
    }.execute()
}

fun startDownloadPic(url: String): ByteArray {
    log("耗时操作，下载图片")
    val response = HttpService.getLogo(url)
    if (response.isSuccessful) {
        response.body()?.bytes()?.let {
            return it
        }
        throw HttpException(HttpError.HTTP_ERROR_NO_DATA)
    } else {
        throw HttpException(response.code())
    }
}

Main.kt
		startMyCoroutine(DownloadContext(LOGO_URL)) {
            log("协程开始")
            val pic = startConsumeTime{
               startDownloadPic(this[DownloadContext]!!.url)
            }
            val bitmap = BitmapFactory.decodeByteArray(pic, 0, pic.size)
            img.setImageBitmap(bitmap)
            log("拿到图片")
        }
```

## 协程流程
协程会被编译成状态机，遇到suspend函数之后，会进行状态转移（这个过程中可能还会遇到协程进行其他的状态转移），线程挂起等待最后的执行结果resume或者抛出异常resumeWithException。



