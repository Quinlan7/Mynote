# UUID

[spring boot学习笔记之UUID](https://blog.csdn.net/Vaingloryss/article/details/89408422)

##### uuid

> UUID（通用唯一标识符）是一种用于标识信息对象的128位二进制值，通常以32个十六进制数字（分为5组，8-4-4-12的格式）表示。UUID在计算机系统中广泛应用，特别是在分布式计算环境中，用于确保生成的标识符在多个节点之间是唯一的。
>
> UUID是由网络计算领域（DCE）工作组在1980年代末和1990年代初创建的，并在之后被多个标准化组织采纳。UUID的目标是在全球范围内保证标识符的唯一性，无论何时何地生成。
>
> UUID的生成算法通常基于时间戳、随机数和计算机的MAC地址等信息，以确保生成的标识符在实践中是唯一的。UUID的唯一性和随机性使其在分布式系统中广泛用于标识数据、节点、事务和其他资源，以便于跟踪和管理。

##### springboot uuid 使用

主键实体类用这个注解

@TableId(type = IdType.UUID)