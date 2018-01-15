# 观察者模式    (发布(publish)-订阅(Subscribe)模式)
> 观察者设计模式定义了对象间的一种**一对多**的组合关系，以便一个对象的状态发生变化时，所有依赖于它的对象都得到通知并自动刷新。

### 定义
1. 观察者(Observer):将自己注册到被观察对象（Subject）中，被观察对象将观察者存放在一个容器（Container）里。
2. 被观察:被观察对象发生了某种变化，从容器中得到所有注册过的观察者，将变化通知观察者。
3. 撤销观察:观察者告诉被观察者要撤销观察，被观察者从容器中将观察者去除。
```
  观察者将自己注册到被观察者的容器中时，被观察者不应该过问观察者的具体类型，而是应该使用观察者的接口。这样的优点是：
假定程序中还有别的观察者，那么只要这个观察者也是相同的接口实现即可。一个被观察者可以对应多个观察者，当被观察者发生
变化的时候，他可以将消息一一通知给所有的观察者。基于接口，而不是具体的实现——这一点为程序提供了更大的灵活性。
```
### 观察者模式的应用

##### 场景
>	气象局需要我们构建一套系统，这系统有两个公告牌，分别用于显示当前的实时天气和未来几天的天气预报。当气象局发布新的天气
数据（WeatherData）后，两个公告牌上显示的天气数据必须实时更新。气象局同时要求我们保证程序拥有足够的可扩展性，因为后
期随时可能要新增新的公告牌。

##### 主题(被观察者)
```
/**
 * 主题(被观察者 发布者publish)
 * @author 	JOE
 *
 */

public interface Subject {
	
	/**
	 * 注册观察者
	 */
	void registerObserver(Observer observer);
	
	/**
	 * 移除观察者
	 */
	void removeObserver(Observer observer);
	
	/**
	 * 通知观察者</br>
	 * 是否含有参数表示通知的观察者(空参这位注册的所有观察者)
	 */
	void notifyObserver();
}

```
##### 观察者
```
/**
 * 观察者
 * @author JOE
 *
 */
public interface Observer {
	
	void update();
}

```

##### 公告牌用于显示的接口
```
/**
 * 公告牌接口
 * @author JOE
 *
 */
public interface DisplayElement {

	void display();
	
}

```

##### 主题的实现(发布的实现)
```
/**
 * 天气主题
 * 
 * @author JOE
 */
public class WeatherSubject implements Subject {

	private List<Observer> observers;

	private float temperature;// 温度
	private float humidity;// 湿度
	private float pressure;// 气压
	private List<Float> forecastTemperatures;// 未来几天的温度

	public WeatherSubject() {
		this.observers = Lists.newArrayList();
	}

	@Override
	public void registerObserver(Observer observer) {
		this.observers.add(observer);
	}

	@Override
	public void removeObserver(Observer observer) {
		this.observers.remove(observer);
	}
	
	@Override
	public <T> void notifyObserver(T t) {
		observers.forEach(o -> {
			o.update(t);
		});
	}

	public void setMeasurements(float temperature, float humidity, float pressure, List<Float> forecastTemperatures) {
		this.temperature = temperature;
		this.humidity = humidity;
		this.pressure = pressure;
		this.forecastTemperatures = forecastTemperatures;
		measureDataChange();
	}

	public void measureDataChange() {
		Map<String, Object> map = Maps.newHashMap();
		map.put("temperature", temperature);
		map.put("humidity", humidity);
		map.put("pressure", pressure);
		map.put("forecastTemperatures", forecastTemperatures);
		notifyObserver(map);
	}

	public List<Observer> getObservers() {
		return observers;
	}

	public float getTemperature() {
		return temperature;
	}

	public float getHumidity() {
		return humidity;
	}

	public float getPressure() {
		return pressure;
	}

	public List<Float> getForecastTemperatures() {
		return forecastTemperatures;
	}

}

```


##### 观察者实现
```
/**
 * 当前天气的显示牌
 * @author JOE
 *
 */
public class CurrentConditionsDisplay implements Observer, DisplayElement {
	
	final static private ObjectMapper mapper = new ObjectMapper();
	
	private float temperature;//温度
    	private float humidity;//湿度
    	private float pressure;//气压

	@Override
	public <T> void update(T t) {
		JsonNode displayData = mapper.convertValue(t, JsonNode.class);
		this.temperature = Float.parseFloat(displayData.get("temperature").asText());
		this.humidity = Float.parseFloat(displayData.get("humidity").asText());
		this.pressure = Float.parseFloat(displayData.get("pressure").asText());
		display();
	}

	@Override
	public void display() {
		System.out.println("当前温度为：" + this.temperature + "℃");
        System.out.println("当前湿度为：" + this.humidity);
        System.out.println("当前气压为：" + this.pressure);	
	}

}

```

##### 测试
```
/**
 * 观察者模式测试
 * @author JOE
 *
 */
public class PublishSubscribeTest {
	
	@org.junit.Test
	public void test(){
		WeatherSubject weatherSubject = new WeatherSubject();
		weatherSubject.registerObserver(new CurrentConditionsDisplay());
		weatherSubject.setMeasurements(10, 11, 12, Lists.newArrayList(13.0f, 14.0f));
	}
}
```

### java内置观察者模式


