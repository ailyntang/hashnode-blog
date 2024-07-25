---
title: "Azure logs - invocation id vs operation id"
datePublished: Thu Jul 25 2024 04:53:28 GMT+0000 (Coordinated Universal Time)
cuid: clz0ssz1k000p0ajtc4eo3nry
slug: azure-logs-invocation-id-vs-operation-id
tags: dotnet

---

**Function invocation id**

* scoped for the webjob execution
    
* \`Properties.MS\_FunctionInvocationId\`
    

**Operation id**

* scoped for the lifetime of the http request