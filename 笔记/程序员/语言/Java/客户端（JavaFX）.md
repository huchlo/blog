
[JavaFX中文官方网站 (openjfx.cn)](https://openjfx.cn/)

# 开始
入口参数：
```java
public class JavaFXLoginDemo extends Application {

    @Override
    public void start(Stage primaryStage) {
    }
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        launch(args);
    }
}
```
只有一个按钮需要事件处理：
```java
btn.setOnAction(new EventHandler<ActionEvent>() {//注册事件handler
	@Override
	public void handle(ActionEvent e) {
		actiontarget.setFill(Color.FIREBRICK);//将文字颜色变成 firebrick red
		actiontarget.setText("登录中...");
	}
});
```

# fxml嵌套
所有控件都是在stage上显示，最上层的stage由底层平台提供。
比如Application提供，或者Swing接口JFXPanel提供。
读取fxml后产生的节点Node需要放在一个场景Scene中，而场景Scene要放在某个舞台stage上。
舞台stage由平台(Application, Swing, SWT)产生和提供

在控制器里面

	Stage window = new Stage();
	Parent root = FXMLLoader.load(getClass().getResource("pagelist.fxml"));
	Scene scene = new Scene(root);
	window.setTitle("My pagelist");
	window.setScene(scene);
	window.show();
	window.showAndWait(); //使用showAndWait()先处理这个窗口，而如果不处理，main中的那个窗口不能响应


