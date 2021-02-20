# Memento

备忘录，记录状态，方便回滚

![image-20201217203312462](Memento.assets/image-20201217203312462.png)

序列化需要实现serilzable接口，且每个引用属性都要实现 ，如果有不想序列化的属性可以使用transient关键字