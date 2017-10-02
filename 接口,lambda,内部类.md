# 接口

- 接口绝不能含有`实例域`,接口中的方法也不能引用实例域,**接口没有实例**,提供实例域和方法实现的任务应该由实现接口的那个类来完成
- 在实现接口时**必须**把方法声明为`public`,否则编译器将认为这个方法的访问属性是包可见性
- 接口中可以包含**常量**,接口中的域将自动被设为`public static final`
- 接口可以为方法提供一个默认实现,必须用`default`标记这样的方法,主要用于为暂时没用的方法设空方法

```java
public interface MouseListener {
    default void mouseClicked(MouseEvent event){}
}
```

## 回调

- 通过回调指出某个特定事件发生时应该采取的动作

```java
public class TimerTest {
    public static void main(String[] args) {
        //向上转型
        ActionListener listener = new TimerPrinter();

        Timer t = new Timer(1000, listener);
        t.start();

    }
}

class TimerPrinter implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println(" " + new Date());
        Toolkit.getDefaultToolkit().beep();
    }
}
```

## lambda表达式

- `(参数)->{逻辑表达式}`,如果可以推导出参数类型,就可以忽略参数类型
- 需要将一个代码块传递到某个对象(一个定时器或是一个sort方法),这个代码块在将来某个时间被调用

### 函数式接口

- 对于**只有一个抽象方法**的接口,需要这种接口的对象时就可以提供一个lambda表达式,这种接口称为函数式接口

```java
public class TimerTest {
    public static void main(String[] args) {
        //向上转型
        ActionListener listener = new TimerPrinter();

        Timer t1 = new Timer(1000, listener);

        Timer t2 = new Timer(1000, event-> {
            System.out.println(new Date());
            Toolkit.getDefaultToolkit().beep();
        });

        Timer t3 = new Timer(1000, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println(new Date());

            }
        });

        t1.start();

    }
}

class TimerPrinter implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println(" " + new Date());
        Toolkit.getDefaultToolkit().beep();
    }
}
```

### 方法引用

- 条件:已经有**现成的方法**可以完成你想要传递到其他代码的某个动作
- 用`::`操作符分隔方法名与对象或类名,省略参数

```java
public static void main(String[] args) {
       String strings[] = {};
       Arrays.sort(strings, String::compareToIgnoreCase);

       Arrays.sort(strings,(x,y)->x.compareToIgnoreCase(y));


       Timer t = new Timer(1000, event -> System.out.println(event));
       Timer t1 = new Timer(1000, System.out::println);
   }
```

### 构造器引用

- 构造器引用与方法引用类似,不过方法名为`new`
