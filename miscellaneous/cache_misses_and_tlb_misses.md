# 前言
+ Garena和现在雷霆的项目，ds都是ECS架构。在我看来ECS架构最少有两个优势，第一，在架构层面通过system与component的解耦，使各个system仅关注自身逻辑，避免了行为之间的直接耦合。第二，在采用以component为中心的内存布局时，system能够顺序访问其所需的组件数据，能够显著提升cache line利用率，减少随机内存访问带来的cache miss和tlb miss。

# 测试
+ 
  通过对一个int类型数组进行顺序访问和跨页访问，比较两种情况下的cache misses和IPC的值，理论上顺序访问的cache misses更少，IPC更大。但此时IPC的差异不单单是cache misses导致的，顺序访问还会有更少的tlb misses，但测试环境是虚拟机，perf观察不到tlb misses相关数据。  
  之所以是跨页访问，而不是逆序访问，是因为逆序访问也会触发cpu prefetch，只是相较于顺序访问规则更难触发而已，而跨页访问基本能确保每次都会触发cache misses。