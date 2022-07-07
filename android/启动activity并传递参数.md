# 启动activity并传递参数

```java
btn.setOnClickListener(new View.OnClickListener(){
    @Override
    public void onClick(View v){
        Intent i = new Intent(MainActivity.this, theAty.class);
        i.putExtra("data", "hello");
        startActivity(i);
    }
})
```



使用bundle传递参数

```java
btn.setOnClickListener(new View.OnClickListener(){
    @Override
    public void onClick(View v){
        Intent i = new Intent(MainActivity.this, theAty.class);
        Bundle b = new Bundle();
        b.putString("name", "hello");
        b.putInt("age", 13);
        i.putExtras(b);
        // 或者直接使用bundle传递
        // i.putExtra("data", b);
        startActivity(i);
    }
})
```



使用bundle获取参数

```java
@Override
protected void onCreate(Bundle saveInstanceState){
    super.onCreate(saveInstanceState);
    setContentView(R.layout.activity_the_aty);
    
    Intent i = getIntent();
    Bundle data = i.getExtras();
    System.out.println("name: " + data.getString("name") + "  age: " + data.getInt("age"));
}
```





使用intent传递自定义对象，要求对象的类实现java.io.Serializable 接口 ，或是android.os.Parcelable接口

android.os.Parcelable接口要求类重写describeContents 、 writeToParcel方法 以及定义类方法

```java
public class User implements Parcelable{
    private String name;
    private int age;
    public User(String name, int age){
        this.name = name;
        this.age = age;
    }
    
    public String getName(){
        return name;
    }
    
    public int getAge(){
        return age;
    }
    
    @Override
    public int describeContents(){
        return 0;
    }
    
    @Override
    public void writeToParcel(Parcel dest, int flags){
        dest.writeString(getName());
        dest.writeInt(getInt());
    }
    // 如果有多个相同类型的成员，可以使用source.writeBundle(Bundle val)进行createFromParcel
    public static final Creator<User> CREATOR = new Creator<User>(){
        @Override
        public User createFromParcel(Parcel source){
            return new User(source.readString(), source.readInt());
        }
        
        @Override
        public User[] newArray(int size){
            return new User[size];
        }
    }
    
}
```





获取activity的返回参数

```java
// 第一个activity 获取第二个activity的返回参数
btn.setOnClickListener(new View.OnClickListener(){
    @Override
    public void onClick(View v){
        Intent i = new Intent(MainActivity.this, theAty.class);
        i.putExtra("data", "hello");
        startActivityForResult(i, 0);
    }
});

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data){
    super.onActivityResult(requestCode, resultCode);
    textView.setText(data.getStringExtra("data"));
}

```



```java
// 第二个activity，用于被第一个activity启动后，返回参数
btn.setOnClickListener(new View.OnClickListener(){
    @Override
    public void onClick(View v){
        Intent i = new Intent();
        i.putExtra("data", editText.getText().toString());
        // setResult第一个参数1表示成功
        setResult(1, i);
        finish();
    }
})
```





















