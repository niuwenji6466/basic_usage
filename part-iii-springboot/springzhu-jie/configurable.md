我们想在非Spring管理的类中使用依赖注入;比如：手动new出来的对象，正常情况下，Spring是无法依赖注入的，这个时候可以使用@Configurable注解。

