# 创建用户
```mongodb-json
//进入amdin数据库
use admin
db.createUser(
    {
        user:"root",
        pwd:"123456",
        // 在admin数据库中通过创建一个用户赋予用户root权限
        roles:[{role:"root",db:"admin"}] 
    }
)

db.auth("root","123456")
```

> 创建admin超级管理员用户
```mongodb-json
use admin
db.createUser(  
  { user: "admin",  
    customData：{description:"superuser"},
    pwd: "admin",  
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]  
  }  
)
db.auth("admin","admin")
```

> 创建一个不受访问限制的超级用户
```mongodb-json
use admin
db.createUser(  
  { user: "admin",  
    pwd: "admin",  
    roles: [ { role: "root", db: "admin" } ]  
  }  
)
db.auth("admin","admin")
```

> 创建一个业务数据库管理员用户
```mongodb-json
db.createUser({
    user:"user001",
    pwd:"123456",
    customData:{
        name:'jim',
        email:'jim@qq.com',
        age:18,
    },
    roles:[
        {role:"readWrite",db:"db001"},
        {role:"readWrite",db:"db002"},
       'read'// 对其他数据库有只读权限，对db001、db002是读写权限
    ]
})
```

> MongoDB数据库角色说明
- 数据库用户角色：read、readWrite； 
- 数据库管理角色：dbAdmin、dbOwner、userAdmin; 
- 集群管理角色：clusterAdmin、clusterManager、4. clusterMonitor、hostManage； 
- 备份恢复角色：backup、restore； 
- 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase 
- 超级用户角色：root 
- 内部角色：__system

> MongoDB中的role详解
- Read：允许用户读取指定数据库 
- readWrite：允许用户读写指定数据库 
- dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
-  userAdmin：允许用户向system.users集合写入，可以在指定数据库里创建、删除和管理用户
 - clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限
 - readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
 - readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
 - userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
 - dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限
 - root：只在admin数据库中可用。超级账号，超级权限
