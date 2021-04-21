# npapi on chromium 

## xembed
[xembed protocol](https://www.freedesktop.org/wiki/Specifications/xembed-spec/)


## script

## multiprocess

## 接口文件

npapi.h 数据类型，以及OOP编程 的接口约定。
npfuntions.h  
    plugin 提供給浏览器的接口： NPPluginFuncs.      browser 提供給插件的接口 NPNetscapeFuncs
    调用时，需要加上Instance信息
npruntime.h
  提供了c语言调用脚本（js）环境的约定。它并没有说明user agent的功能。
    This API is used to facilitate binding code written in C to script
    objects.  The API in this header does not assume the presence of a
    user agent.  That is, it can be used to bind C code to scripting
    environments outside of the context of a user agent.
    
    However, the normal use of the this API is in the context of a
    scripting environment running in a browser or other user agent.
    In particular it is used to support the extended Netscape
    script-ability API for plugins (NP-SAP).  NP-SAP is an extension
    of the Netscape plugin API.  As such we have adopted the use of
    the "NP" prefix for this API.

    The following NP{N|P}Variables were added to the Netscape plugin
    API (in npapi.h):

    NPNVWindowNPObject
    NPNVPluginElementNPObject
    NPPVpluginScriptableNPObject

    These variables are exposed through NPN_GetValue() and
    NPP_GetValue() (respectively) and are used to establish the
    initial binding between the user agent and native code.  The DOM
    objects in the user agent can be examined and manipulated using
    the NPN_ functions that operate on NPObjects described in this
    header.

    To the extent possible the assumptions about the scripting
    language used by the scripting environment have been minimized.


    NPObjects returned by create, retain, invoke, and getProperty pass
    a reference count to the caller.  That is, the callee adds a
    reference count which passes to the caller.  It is the caller's
    responsibility to release the returned object.

    NPInvokeFunctionPtr function may return 0 to indicate a void
    result.

    NPInvalidateFunctionPtr is called by the scripting environment
    when the native code is shutdown.  Any attempt to message a
    NPObject instance after the invalidate callback has been
    called will result in undefined behavior, even if the native code
    is still retaining those NPObject instances.  (The runtime
    will typically return immediately, with 0 or NULL, from an attempt
    to dispatch to a NPObject, but this behavior should not be
    depended upon.)

    The NPEnumerationFunctionPtr function may pass an array of
    NPIdentifiers back to the caller. The callee allocs the memory of
    the array using NPN_MemAlloc(), and it's the caller's responsibility
    to release it using NPN_MemFree().



nptypes.h



## 变量/函数

## npapi规范的问题


## browser process


## NPAPI 实例管理



## chromium npapi 实现
npobject_proxy  
    1. 在插件进程，实现浏览器提供给插件的接口【NPN】。
    2. 在浏览器进程，实现浏览器对插件的调用【发送IPC消息】。





## 当前实现方案的问题
chromium 不支持 browser plugin ？？？？


## 参考文档

1. https://www.codeproject.com/Articles/92787/Working-on-an-NPAPI-browser-plugin
