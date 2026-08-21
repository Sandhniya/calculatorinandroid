## EX:NO:05:Develop a program to create a simple calculator using android studio.
## Aim:
To create and design an android application for a simple calculator using android studio.
## EQUIPMENTS REQUIRED:
Android Studio(Latest Version)
## ALGORITHM:
Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as smsintent and click Next.

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6:Display details give in MainActivity file.

Step 7: Save and run the application.
## PROGRAM:
Program to create and design an android application simple calculator using Intent.
### Developed by: SANDHIYA SREE
### Register Number :212223220093

## AndroidMainfest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Calculator">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
## MainActivity.java
```
package com.example.calculator;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private TextView tvDisplay;
    private StringBuilder inputExpression = new StringBuilder();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvDisplay = findViewById(R.id.tvDisplay);

        int[] buttonIds = {
                R.id.btnAC, R.id.btnPercent, R.id.btnBackspace, R.id.btnDivide,
                R.id.btn7, R.id.btn8, R.id.btn9, R.id.btnMultiply,
                R.id.btn4, R.id.btn5, R.id.btn6, R.id.btnMinus,
                R.id.btn1, R.id.btn2, R.id.btn3, R.id.btnPlus,
                R.id.btn00, R.id.btn0, R.id.btnDot, R.id.btnEquals
        };

        for (int id : buttonIds) {
            findViewById(id).setOnClickListener(this);
        }
    }

    @Override
    public void onClick(View v) {
        Button btn = (Button) v;
        String text = btn.getText().toString();

        switch (text) {
            case "AC":
                inputExpression.setLength(0);
                break;
            case "⌫":
                if (inputExpression.length() > 0) {
                    inputExpression.deleteCharAt(inputExpression.length() - 1);
                }
                break;
            case "=":
                calculateResult();
                return;
            default:
                inputExpression.append(text);
                break;
        }
        tvDisplay.setText(inputExpression.toString());
    }

    private void calculateResult() {
        String expr = inputExpression.toString()
                .replace("÷", "/")
                .replace("×", "*");
        try {
            // Evaluates simple arithmetic expression
            double result = evaluateSimpleExpression(expr);
            if (result == (long) result) {
                tvDisplay.setText(String.format("%d", (long) result));
            } else {
                tvDisplay.setText(String.valueOf(result));
            }
            inputExpression.setLength(0);
            inputExpression.append(tvDisplay.getText());
        } catch (Exception e) {
            tvDisplay.setText("Error");
            inputExpression.setLength(0);
        }
    }

    private double evaluateSimpleExpression(String expression) {
        // Simple evaluator logic (Supports basic operations)
        return new Object() {
            int pos = -1, ch;

            void nextChar() {
                ch = (++pos < expression.length()) ? expression.charAt(pos) : -1;
            }

            boolean eat(int charToEat) {
                while (ch == ' ') nextChar();
                if (ch == charToEat) {
                    nextChar();
                    return true;
                }
                return false;
            }

            double parse() {
                nextChar();
                double x = parseExpression();
                if (pos < expression.length()) throw new RuntimeException("Unexpected: " + (char)ch);
                return x;
            }

            double parseExpression() {
                double x = parseTerm();
                for (;;) {
                    if      (eat('+')) x += parseTerm();
                    else if (eat('-')) x -= parseTerm();
                    else return x;
                }
            }

            double parseTerm() {
                double x = parseFactor();
                for (;;) {
                    if      (eat('*')) x *= parseFactor();
                    else if (eat('/')) x /= parseFactor();
                    else if (eat('%')) x %= parseFactor();
                    else return x;
                }
            }

            double parseFactor() {
                if (eat('+')) return parseFactor();
                if (eat('-')) return -parseFactor();

                double x;
                int startPos = this.pos;
                if ((ch >= '0' && ch <= '9') || ch == '.') {
                    while ((ch >= '0' && ch <= '9') || ch == '.') nextChar();
                    x = Double.parseDouble(expression.substring(startPos, this.pos));
                } else {
                    throw new RuntimeException("Unexpected character");
                }
                return x;
            }
        }.parse();
    }
}
```
## styles.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources><style name="CalcBtn">
    <item name="android:layout_width">0dp</item>
    <item name="android:layout_height">72dp</item>
    <item name="android:layout_columnWeight">1</item>
    <item name="android:layout_margin">8dp</item>
    <item name="android:background">@drawable/btn_circle_dark</item>
    <item name="android:textColor">#FFFFFF</item>
    <item name="android:backgroundTint">@null</item>
    <item name="android:textSize">24sp</item>
    <item name="android:textStyle">bold</item>
</style>

    <drawable name="btn_circle_dark" />
</resources>
```
### colours.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>
```
### activity.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#17171C"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- Display Area -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="bottom|end"
        android:orientation="horizontal"
        android:padding="16dp">

        <TextView
            android:id="@+id/tvDisplay"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FFFFFF"
            android:textSize="48sp" />

        <!-- Cursor line matching the screenshot -->
        <View
            android:layout_width="3dp"
            android:layout_height="48dp"
            android:layout_marginStart="4dp"
            android:background="#FF9500" />
    </LinearLayout>

    <!-- Keypad Grid -->
    <GridLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:columnCount="4"
        android:rowCount="5">

        <!-- Row 1 -->
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnAC"
            android:text="AC" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnPercent"
            android:text="%" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnBackspace"
            android:text="⌫" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnDivide"
            android:text="÷" />

        <!-- Row 2 -->
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn7"
            android:text="7" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn8"
            android:text="8" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn9"
            android:text="9" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnMultiply"
            android:text="×" />

        <!-- Row 3 -->
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn4"
            android:text="4" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn5"
            android:text="5" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn6"
            android:text="6" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnMinus"
            android:text="-" />

        <!-- Row 4 -->
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn1"
            android:text="1" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn2"
            android:text="2" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn3"
            android:text="3" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnPlus"
            android:text="+" />

        <!-- Row 5 -->
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn00"
            android:text="00" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btn0"
            android:text="0" />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnDot"
            android:text="." />
        <android.widget.Button
            style="@style/CalcBtn"
            android:id="@+id/btnEquals"
            android:background="@drawable/orangecircle"
            android:text="=" />
    </GridLayout>

</LinearLayout>
```
### output
<img width="1601" height="810" alt="image" src="https://github.com/user-attachments/assets/e00f45d6-28c8-45b7-b84b-e0825c5f7aa0" />

<img width="1610" height="813" alt="image" src="https://github.com/user-attachments/assets/cd74ba60-2866-47ee-bfcb-713699fc9153" />





## RESULT
Thus a Simple Android Application create a simple calculator using Android Studio is developed and executed successfully.
