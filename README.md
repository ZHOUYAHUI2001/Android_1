
package cn.edu.bistu.cs.calculater;
import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends Activity {

    private EditText output = null;
    private EditText input = null;

    public String FILE_NAME = "fileDemo.txt";


    private String str = "";//保存数字
    private String strold = "";//原数字
    private char act = ' ';//记录“加减乘除等于”符号
    private int count = 0;//判断要计算的次数，如果超过一个符号，先算出来一部分
    private Float result = null;//计算的输出结果
    private Boolean errBoolean = false;//有错误的时候为true，无错为false
    private Boolean flagBoolean = false;//一个标志，如果为true，可以响应运算消息，如果为false，不响应运算消息，只有前面是数字才可以响应运算消息
    private Boolean flagDot = false; //小数点标志位



    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        output = findViewById(R.id.output);
        input = findViewById(R.id.input);

        Button btn0 = findViewById(R.id.zero);
        Button btn1 = findViewById(R.id.one);
        Button btn2 = findViewById(R.id.two);
        Button btn3 = findViewById(R.id.three);
        Button btn4 = findViewById(R.id.four);
        Button btn5 = findViewById(R.id.five);
        Button btn6 = findViewById(R.id.six);
        Button btn7 = findViewById(R.id.seven);
        Button btn8 = findViewById(R.id.eight);
        Button btn9 = findViewById(R.id.nine);
        Button btnadd = findViewById(R.id.add);
        Button btnsubtract = findViewById(R.id.subtract);
        Button btnmultiply = findViewById(R.id.multiply);
        Button btndivide = findViewById(R.id.divide);
        Button btnclear = findViewById(R.id.clear);
        Button btnresult = findViewById(R.id.result);
        Button btndot = findViewById(R.id.dot);
        Button btnbaifenhao = findViewById(R.id.baifenhao);
        Button btnqufan = findViewById(R.id.qufan);
        Button btngenhao = findViewById(R.id.genhao);
        //设置按钮侦听事件
        btn0.setOnClickListener(listener);
        btn1.setOnClickListener(listener);
        btn2.setOnClickListener(listener);
        btn3.setOnClickListener(listener);
        btn4.setOnClickListener(listener);
        btn5.setOnClickListener(listener);
        btn6.setOnClickListener(listener);
        btn7.setOnClickListener(listener);
        btn8.setOnClickListener(listener);
        btn9.setOnClickListener(listener);
        //执行运算
        btnadd.setOnClickListener(listener);
        btnsubtract.setOnClickListener(listener);
        btnmultiply.setOnClickListener(listener);
        btndivide.setOnClickListener(listener);
        btnclear.setOnClickListener(listener);
        btnresult.setOnClickListener(listener);
        btnbaifenhao.setOnClickListener(listener);
        btnqufan.setOnClickListener(listener);
        btngenhao.setOnClickListener(listener);
        btndot.setOnClickListener(listener);

    }


    private final OnClickListener listener = new OnClickListener() {

        public void onClick(View v) {
            // TODO Auto-generated method stub
            switch (v.getId()) {
                //输入数字
                case R.id.zero:
                    num(0);
                    break;
                case R.id.one:
                    num(1);
                    break;
                case R.id.two:
                    num(2);
                    break;
                case R.id.three:
                    num(3);
                    break;
                case R.id.four:
                    num(4);
                    break;
                case R.id.five:
                    num(5);
                    break;
                case R.id.six:
                    num(6);
                    break;
                case R.id.seven:
                    num(7);
                    break;
                case R.id.eight:
                    num(8);
                    break;
                case R.id.nine:
                    num(9);
                    break;
                case R.id.dot:
                    dot();
                    break;

                //执行运算
                case R.id.add:
                    add();
                    break;
                case R.id.subtract:
                    sub();
                    break;
                case R.id.multiply:
                    multiply();
                    break;
                case R.id.divide:
                    divide();
                    break;
                case R.id.baifenhao:
                    baifenhao();
                    break;
                case R.id.qufan:
                    qufan();
                    break;
                case R.id.genhao:
                    genhao();
                    break;
                case R.id.clear:
                    clear();
                    break;
                //计算结果
                case R.id.result:
                    result();
                    break;

                default:
                    break;

            }

            input.setText(strold + act + str);
            output.setText(String.valueOf(result)+"");//第二行的输出


        }
    };

    private void dot() {
        // TODO Auto-generated method stub
        if (!flagDot) {
            str = str + ".";
            flagBoolean = false;
            flagDot = true;
        }
    }

    private void clear() {
        // TODO Auto-generated method stub
        str = strold = "";
        count = 0;
        act = ' ';
        result = null;
        flagBoolean = false;
        flagDot = false;
        input.setText(strold + act + str);
        output.setText("");
    }

    private void divide() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '/';
            flagBoolean = false;
        }
    }
    private void baifenhao() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '%';
            flagBoolean = true;

        }
    }
    private void qufan() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '±';
            flagBoolean = true;

        }
    }
    private void genhao() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '√';
            flagBoolean = true;

        }
    }
    private void multiply() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '*';
            flagBoolean = false;
        }
    }
    private void sub() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '-';
            flagBoolean = false;
        }
    }
    private void add() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            check();
            act = '+';
            flagBoolean = false;
        }
    }
    private void check() {
        // TODO Auto-generated method stub
        if (count >= 1) {
            result();
            str = String.valueOf(result);
        }
        strold = str;
        str = "";
        count++;
        flagDot = false;
    }

    //计算输出结果
    private void result() {
        // TODO Auto-generated method stub
        if (flagBoolean) {
            if(act=='+'||act=='-'||act=='*'||act=='/'){
            Float a, b;
            a = Float.parseFloat(strold);
            b = Float.parseFloat(str);
            if (b == 0 && act == '/') {
                clear();
                Toast.makeText(getApplicationContext(), "除数不得为0", Toast.LENGTH_LONG).show();
                //errBoolean=true;
            }
            if (!errBoolean) {
                switch (act) {
                    case '+':
                        result = a + b;
                        break;
                    case '-':
                        result = a - b;
                        break;
                    case '*':
                        result = a * b;
                        break;
                    case '/':
                        result = a / b;
                        break;
                    default:
                        break;
                }
            }
        }
            else if(act=='%'){
                if(str.equals("")){
                float a;
                a = Float.parseFloat(strold);
                if (!errBoolean) {
                        result =a/100;
                }
                }
                else{
                    clear();
                    Toast.makeText(getApplicationContext(), "%运算输入错误", Toast.LENGTH_LONG).show();
                }

            }
            else if(act=='±'){
                if(str.equals("")){
                    float a;
                    a = Float.parseFloat(strold);
                    if (!errBoolean) {
                        if(act=='±'){
                            result =a*(-1);
                        }
                    }
                }
                else{
                    clear();
                    Toast.makeText(getApplicationContext(), "±运算输入错误", Toast.LENGTH_LONG).show();
                }

            }
            else if(act=='√'){
                if(str.equals("")){
                    float a;
                    a = Float.parseFloat(strold);
                    if(a>=0){
                    if (!errBoolean) {
                        if(act=='√'){
                            result =(float)Math.sqrt(a);
                        }
                    }
                    }
                    else
                        Toast.makeText(getApplicationContext(), "负数不能开平方", Toast.LENGTH_LONG).show();
                }
                else{
                    clear();
                    Toast.makeText(getApplicationContext(), "√运算输入错误", Toast.LENGTH_LONG).show();
                }

            }

        }
        else {
            Toast.makeText(getApplicationContext(), "输入错误", Toast.LENGTH_LONG).show();

        }
    }

    private void num(int i) {
        // TODO Auto-generated method stub
        str = str + i;
        flagBoolean = true;
    }

}
