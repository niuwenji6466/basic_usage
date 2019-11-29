我们想在非Spring管理的类中使用依赖注入;比如：手动new出来的对象，正常情况下，Spring是无法依赖注入的，这个时候可以使用@Configurable注解。

```
public class CarSalon {
    //...
    public void testDrive() {
        Car car = new Car();
        car.startCar();
    }
}

@Component
public class Car {
    @Autowired
    private Engine engine;
    
    @Autowired
    private Transmission transmission;
 
    public void startCar() {
        transmission.setGear(1);
        engine.engineOn();
        System.out.println("Car started");
    }
}
 
@Component
public class Engine { //...
//...
}
 
@Component
public class Transmission {//...
//...
}
```

代码运行时，会把Null异常。因为正常情况下，Spring无法对new出来的对象进行依赖注入;在此基础上，我们使用@Configurable注解进行修改，如下：

```
@Configurable(preConstruction = true)
@Component
public class Car {
 
    @Autowired
    private Engine engine;
    @Autowired
    private Transmission transmission;
 
    public void startCar() {
        transmission.setGear(1);
        engine.engineOn();
 
        System.out.println("Car started");
    }
}
```

讲解：@Configurable\(preConstruction = true\) 这个注解的作用是：告诉Spring在构造函数运行之前将依赖注入到对象中。

