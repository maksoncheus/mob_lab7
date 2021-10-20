**Макаров Максим 803в2.**  
Создадим приложение без activity. Создадим в нем layout и добавим элементы управления:  
```
	<TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="136dp"
        android:layout_marginTop="37dp"
        android:text="здесь будет текст"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btn_save"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="17dp"
        android:layout_marginTop="438dp"
        android:text="Save"
        android:onClick="save_click"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btn_load"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="441dp"
        android:layout_marginEnd="19dp"
        android:text="Load"
        android:onClick="load_click"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/editTextTextPersonName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="101dp"
        android:layout_marginTop="168dp"
        android:layout_marginEnd="100dp"
        android:ems="10"
        android:hint="Введите текст"
        android:inputType="textPersonName"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />
```  
При создании activity получим путь к «приватной» папке проекта, к которой у приложения есть
 доступ по умолчанию(необходимо из-за изменений политики доступа к внутреннему 
 хранилищу на новых версиях API):  
```Java
	String filename = "content.txt";
    File file;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        file = new File(MainActivity.this.getFilesDir(),filename);
    }
```
Напишем обработчики для кнопок:  
- Сохранение в файл:  
```Java
	public void save_click(View view)
    {
        try
        {

            EditText textBox = (EditText) findViewById(R.id.editTextTextPersonName);
            String text = textBox.getText().toString();
            try {
                file.createNewFile();
            }
            catch (IOException ex)
            {
                Toast.makeText(this, ex.getMessage(), Toast.LENGTH_SHORT).show();
            }
            FileOutputStream fos = new FileOutputStream(file);
            fos.write(text.getBytes());
            fos.close();
            Toast.makeText(this, "Текстовый файл успешно сохранён!", Toast.LENGTH_SHORT).show();
        } catch (IOException e)
        {
            e.printStackTrace();
            Toast.makeText(this, e.getMessage(), Toast.LENGTH_SHORT).show();
        }
    }
```  
В ходе выполнения кода попробуем создать файл. 
После этого запишем в него текст из EditText.Если файл уже существует, 
просто запишем в него контент.
- Загрузка из файла:  
```Java
	public void load_click(View view)
    {
        try
        {
            FileInputStream fin = new FileInputStream(file);
            byte[] bytes = new byte[fin.available()];
            fin.read(bytes);
            String text = new String(bytes);
            TextView textView = (TextView) findViewById(R.id.textView);
            textView.setText(text);
            fin.close();
        } catch (IOException ex)
        {
            Toast.makeText(this, ex.getMessage(), Toast.LENGTH_SHORT).show();
        }
    }
```  
Проверим работу на устройстве:  
![image info](/imgs/mob_lab7_1.jpg)  
![image info](/imgs/mob_lab7_2.jpg)