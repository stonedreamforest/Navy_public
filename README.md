<p align="center">
  <img src="https://user-images.githubusercontent.com/16742566/80297653-a1b54d80-87b7-11ea-8758-9590f5989a33.png">
</p>

# Navy_public
轻量级自动分析病毒程序调用上下文、游戏反调试实现技术...

#### 使用
1. 启动`Navy32/64.EXE` 按`alt+a`选择要监控的进程
![image](https://user-images.githubusercontent.com/16742566/80297988-25bd0480-87bb-11ea-9357-18460725856e.png)

#### 快捷键
- `ALT + A`: 打开进程列表
- `CTRL + L`: 清屏

#### json数据库
1. 示例
```json
{
    "supportedFunctions": ["NtCreateProcess", "NtQueryInformationProcess"],// 数据库已支持函数（数据库未支持的未显示在gui
    "NtCreateProcess": {
        "hasResult": true, // 函数是否有返回值
        "paraCount": 8, / 函数参数个数（不包括返回值）
        "paras": {
            "para0": { // 函数返回结果（若无也需要保留该字段
                "type": "NSTATUS", // 类型
                "name": "result", // 名称
                "hasPreValue": false // 是否有预定义值 可参考`DB/NTDLL.JSON -> NtQueryInformationProcess`
            },
            "para1": {// 第一个参数
                "type": "PHANDLE",
                "name": "ProcessHandle",
                "hasPreValue": false
            },
            "para2": { // 第二个参数
                "type": "ACCESS_MASK",
                "name": "DesiredAccess",
                "hasPreValue": false
            },
            "para3": {
                "type": "POBJECT_ATTRIBUTES",
                "name": "ObjectAttributes",
                "hasPreValue": false
            },
            "para4": {
                "type": "HANDLE",
                "name": "ParentProcess",
                "hasPreValue": false
            },
            "para5": {
                "type": "BOOLEAN",
                "name": "InheritObjectTable",
                "hasPreValue": false
            },
            "para6": {
                "type": "HANDLE",
                "name": "SectionHandle",
                "hasPreValue": false
            },
            "para7": {
                "type": "HANDLE",
                "name": "DebugPort",
                "hasPreValue": false
            },
            "para8": {
                "type": "HANDLE",
                "name": "ExceptionPort",
                "hasPreValue": false
            }
        }
    }
}    

```

3. 如果有数据显示类似以下结果
> 函数返回类型 函数结果名称(原值/预定义值(若已设置)) 调用类型 (参数返回类型 参数名称(原值/预定义值(若已设置))[预定义注释], ...)

3.1 *调用前*：未调用`NtQueryInformationProcess`时参数的内容
```cpp
NSTATUS result(无返回值) __stdcall (HANDLE ProcessHandle(0xffffffff), PROCESSINFOCLASS ProcessInformationClass(ProcessBasicInformation)[there is any comments], PVOID ProcessInformation(0x695198), ULONG ProcessInformationLength(0x18), PULONG ReturnLength(0x6951b0))
```

3.2 *调用后*：调用`NtQueryInformationProcess`后参数的内容
```cpp
NSTATUS result(0x0) __stdcall (HANDLE ProcessHandle(0xffffffff), PROCESSINFOCLASS ProcessInformationClass(ProcessBasicInformation)[there is any comments], PVOID ProcessInformation(0x695198), ULONG ProcessInformationLength(0x18), PULONG ReturnLength(0x6951b0))
```


#### 已支持函数
- *ntdll.dll*
- [x] NtCreateProcess
- [x] NtCreateProcessEx
- [x] NtOpenProcess
- [x] NtTerminateProcess
- [x] NtSuspendProcess
- [x] NtResumeProcess
- [x] NtQueryInformationProcess
- [x] NtGetNextProcess
- [x] NtGetNextThread
- [x] NtSetInformationProcess
- [x] NtQueryPortInformationProcess
- [x] NtCreateThread
- [x] NtOpenThread
- [x] NtTerminateThread
- [x] NtSuspendThread
- [x] NtResumeThread
- [x] NtGetCurrentProcessorNumber
- [x] NtGetContextThread
- [x] NtSetContextThread
- [x] NtQueryInformationThread
- [x] NtSetInformationThread
- [x] NtAlertThread
- [x] NtAlertResumeThread
- [x] NtImpersonateThread
- [x] NtTestAlert
- [x] NtRegisterThreadTerminatePort
- [x] NtSetLdtEntries
- [x] NtQueueApcThread
- [x] NtQueueApcThreadEx
- [x] NtCreateUserProcess
- [x] NtCreateThreadEx
- [x] NtOpenJobObject
- [x] NtCreateJobObject
- [x] NtAssignProcessToJobObject
- [x] NtTerminateJobObject
- [x] NtIsProcessInJob
- [x] NtQueryInformationJobObject
- [x] NtSetInformationJobObject
- [x] NtCreateJobSet
- [x] NtCreateFile
- [x] NtCreateNamedPipeFile
- [x] NtCreateMailslotFile
- [x] NtOpenFile
- [x] NtDeleteFile
- [x] NtFlushBuffersFile
- [x] NtQueryInformationFile
- [x] NtSetInformationFile
- [x] NtQueryDirectoryFile
- [x] NtQueryEaFile
- [x] NtSetEaFile
- [x] NtQueryQuotaInformationFile
- [x] NtSetQuotaInformationFile
- [x] NtQueryVolumeInformationFile
- [x] NtSetVolumeInformationFile
- [x] NtCancelIoFile
- [x] NtCancelIoFileEx
- [x] NtCancelSynchronousIoFile
- [x] NtDeviceIoControlFile
- [x] NtFsControlFile
- [x] NtReadFile
- [x] NtWriteFile
- [x] NtReadFileScatter
- [x] NtWriteFileGather
- [x] NtLockFile
- [x] NtUnlockFile
- [x] NtQueryAttributesFile
- [x] NtQueryFullAttributesFile
- [x] NtNotifyChangeDirectoryFile
- [x] NtLoadDriver
- [x] NtUnloadDriver
- [x] NtCreateIoCompletion
- [x] NtOpenIoCompletion
- [x] NtQueryIoCompletion
- [x] NtSetIoCompletion
- [x] NtSetIoCompletionEx
- [x] NtRemoveIoCompletion
- [x] NtRemoveIoCompletionEx
- [x] NtAllocateVirtualMemory
- [x] NtFreeVirtualMemory
- [x] NtReadVirtualMemory
- [x] NtWriteVirtualMemory
- [x] NtProtectVirtualMemory
- [x] NtQueryVirtualMemory
- [x] NtLockVirtualMemory
- [x] NtUnlockVirtualMemory
- [x] NtCreateSection
- [x] NtOpenSection
- [x] NtMapViewOfSection
- [x] NtUnmapViewOfSection
- [x] NtExtendSection
- [x] NtQuerySection
- [x] NtAreMappedFilesTheSame
- [x] NtMapUserPhysicalPages
- [x] NtMapUserPhysicalPagesScatter
- [x] NtAllocateUserPhysicalPages
- [x] NtFreeUserPhysicalPages
- [x] NtOpenSession
- [x] NtGetWriteWatch
- [x] NtResetWriteWatch
- [x] NtCreatePagingFile
- [x] NtFlushInstructionCache
- [x] NtFlushWriteBuffer
- [x] NtCreateEnclave
- [x] NtLoadEnclaveData
- [x] NtInitializeEnclave
- [x] NtTerminateEnclave
- [x] NtCallEnclave



#### 示例
![image](https://user-images.githubusercontent.com/16742566/80297627-6a46a100-87b7-11ea-963d-52a59eb95b0a.png)




#### 计划列表
1. 支持多进程、支持64位、更多模块（kener32、user32...） 更多api支持、支持脚本自动拦截并设置参数及返回值

#### 依赖
1. [qt5.14.1](https://www.qt.io/blog/qt-5.14.1-released)
2. [simdjson](https://github.com/simdjson/simdjson)

...

#### 其它
- [下载：v20200429](https://github.com/stonedreamforest/Navy_public/releases/tag/v20200429)
- [更新日志](https://github.com/stonedreamforest/Navy_public/blob/master/CHANGELOG.md)


