# builder设计模式



> Builder模式是一种创建型设计模式，用于将复杂对象的构建与表示分离开来，从而使同样的构建过程可以创建不同的表示。
>
> 在Builder模式中，定义一个Builder接口和一个ConcreteBuilder类，Builder接口规定了必须实现的方法，而ConcreteBuilder则实现了这些方法并完成了复杂对象的创建。此外，还有一个Director类，负责调用ConcreteBuilder中的方法来构建复杂对象。
>
> Builder模式的优点包括：
>
> 1. 可以更好地控制复杂对象的创建过程；
> 2. 可以方便地改变对象的内部表示；
> 3. 可以将对象创建过程与其表示分离，从而使代码更加灵活。
>
> Builder模式的缺点包括：
>
> 1. 需要定义较多的类；
> 2. 对象构建过程需要更多的代码。
>
> 在Java中，很多类库都使用了Builder模式，例如StringBuilder、DocumentBuilder等。此外，Spring Framework中的BeanDefinitionBuilder也是一种Builder模式的应用。