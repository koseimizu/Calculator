# Calculator
# I created a calculate system with Java.
# This is a first work,so it is not perfect.


import javax.swing.*;
import java.awt.BorderLayout;
import java.awt.GridLayout;
import java.awt.event.*;

//mainクラス
public class Calculator 
{
	public static void main(String[] args)
	{
		Dentaku myCalculator = new Dentaku("水越康成の電卓");
		myCalculator.setBounds(100, 100, 500, 250);
		myCalculator.setVisible(true);
	}
}

class Dentaku extends JFrame
{
	//labelは数値表示部（電卓の液晶表示部に相当）
	JLabel label = new JLabel("0");
	
	//数字ボタン
	numBTN bt0 = new numBTN("0");
	numBTN bt1 = new numBTN("1");
	numBTN bt2 = new numBTN("2");
	numBTN bt3 = new numBTN("3");
	numBTN bt4 = new numBTN("4");
	numBTN bt5 = new numBTN("5");
	numBTN bt6 = new numBTN("6");
	numBTN bt7 = new numBTN("7");
	numBTN bt8 = new numBTN("8");
	numBTN bt9 = new numBTN("9");
	numBTN btten = new numBTN(".");

	//演算子ボタン
	funBTN btadd = new funBTN("+");
	funBTN btsub = new funBTN("-");
	funBTN btmul = new funBTN("×");
	funBTN btdiv = new funBTN("÷");
	funBTN bteq = new funBTN("=");
	funBTN btcle = new funBTN("C");
	funBTN btsqr = new funBTN("²√x");
	funBTN btpwo = new funBTN("x^2");
	funBTN btper = new funBTN("%");
	funBTN btcub=new funBTN("x^3");
	funBTN btfac=new funBTN("x!");
	funBTN btexp=new funBTN("e^x");
	funBTN btsin=new funBTN("sin");
	funBTN btcos=new funBTN("cos");
	funBTN bttan=new funBTN("tan");
	funBTN btln=new funBTN("ln");
	funBTN btlog10=new funBTN("log₁₀x");
	funBTN btpie=new funBTN("π");
	funBTN btE=new funBTN("e");
	funBTN bt10x=new funBTN("10^x");
	funBTN btsinh=new funBTN("sinh");
	funBTN btcosh=new funBTN("cosh");
	funBTN bttanh=new funBTN("tanh");
	funBTN btcbr=new funBTN("³√x");

	 //電卓計算用変数を一括宣言
	double operand1, operand2; //第一被演算子,第二被演算子
	String operator = ""; //演算子
	int pointFlag = 0; //小数点が押されたかどうかのフラグ
	int eqflag = 0; //=ボタンが押されたかどうかのフラグ

	Dentaku(String str) 
	{
		//コンストラクタ
		//フレーム名を生成、電卓を構成するメソッドを呼び出す
		super(str);
		createDentaku();
	}
	void createDentaku() 
	{
		JPanel display = new JPanel(); //電卓の表示部
		JPanel body = new JPanel(); //表示部以外の本体部
		JPanel numPanel = new JPanel(); //数値ボタンを配置
		JPanel funPanel = new JPanel(); //機能ボタンを配置 
		

		body.add(numPanel, BorderLayout.CENTER);
		body.add(funPanel, BorderLayout.EAST);
		getContentPane().add(display, BorderLayout.NORTH);
		getContentPane().add(body, BorderLayout.CENTER);

		setDisplay(display); //displayパネル実装メソッドを呼び出す
		setNumPanel(numPanel); //numPanelパネル実装メソッドを呼び出す
		setFunPanel(funPanel); //funPanelパネル実装メソッドを呼び出す 
	}
	void setDisplay(JPanel p) 
	{
		//displayパネルを実装、ラベルを張り付ける
		p.add(label);
	}
	void setNumPanel(JPanel p) 
	{
		//numPanelに数字ボタンを配置
		p.setLayout(new GridLayout(5, 4));
		p.add(bt10x);
		p.add(btE);
		p.add(btpie);
		p.add(bt1);
		p.add(bt2);
		p.add(bt3);
		p.add(bt4);
		p.add(bt5);
		p.add(bt6);
		p.add(bt7);
		p.add(bt8);
		p.add(bt9);
		p.add(bt0);
		p.add(btten);
		p.add(btcle);
	}
	void setFunPanel(JPanel p)
	 {
		//funPanelに演算子ボタンを配置
		p.setLayout(new GridLayout(5, 3));
		p.add(btdiv);
		p.add(btmul);
		p.add(btsub);
		p.add(btadd);
		p.add(bteq);
		p.add(btpwo);
		p.add(btcub);
		p.add(btsqr);
		p.add(btcbr);
		p.add(btper);
		p.add(btfac);
		p.add(btexp);
		p.add(btsin);
		p.add(btcos);
		p.add(bttan);
		p.add(btsinh);
		p.add(btcosh);
		p.add(bttanh);
		p.add(btln);
		p.add(btlog10);
	} 

	//数値ボタン専用クラス：numBTN
	class numBTN extends JButton implements ActionListener
	 {
		public numBTN(String label)
		{
			super(label);
			//ボタンにActionListnenerを付ける
			this.addActionListener(this);
		}
		public void actionPerformed(ActionEvent e) 
		{
			if (eqflag == 1) 
			{ //フラグが1であれば、label値をリセット、フラグを伏せる
				label.setText("0");
				eqflag = 0;
			}
			//クリックされたボタンのラベルを取得
			String str = this.getText();
			//labelの値を変える
			if (label.getText() != "0") 
			{ //ラベル値が0でない場合、文字列の最後に追加
				if (str == ".") 
				{
					//小数点クリックされたときに処理
					if (pointFlag == 1) 
					{
						//小数点すでに存在する場合
						label.setText(label.getText());
					}
					else 
					{
						//小数点まだ存在しない場合
						label.setText(label.getText() + str);
						pointFlag = 1;
					}

				} 
				else 
				{
					//labelの値を変える
					label.setText(label.getText() + str);
				}

			} 
			else 
			{
				if (str != ".") 
				{ //ラベル値が0、小数点ではない場合、文字列を上書き

					label.setText(str);
				} 
				else 
				{ //ラベル値が0、小数点の場合、文字列の最後に追加
					label.setText(label.getText() + str);
					pointFlag = 1;
				}

			}
		}
	}

	//演算子ボタン専用クラス:funBTN
	class funBTN extends JButton implements ActionListener 
	{
		funBTN(String label) 
		{
			super(label);
			//ボタンにActionListenerを付ける
			this.addActionListener(this);
		}
		public void actionPerformed(ActionEvent e) 
		{
			//クリックされた演算子ボタンのラベルを取得
			String str = this.getText();
			//記録中のoperator値を調査、記録がなければlabelの値をoperand1に記録すればよい
			if (operator == "") 
			{
				//operand1にlabelの値（文字列）を実数変換して保存
				operand1 = Double.valueOf(label.getText());
				//operatorに演算子を記録
				operator = str;
				if (str == "=") 
				{
					label.setText(label.getText());
				} 
				else 
				{
					label.setText("0");
				}

			}
			//記録中のoperator値があれば、つまりoperand1にすでに値がある、
			//labelの値は第二被演算子に該当するからoperand2に記録
			else 
			{
				operand2 = Double.valueOf(label.getText());
				if (operator=="+") 
				{
					operand1=operand1 + operand2;
				}
				if (operator=="-") 
				{
					operand1=operand1 - operand2;
				}
				if (operator=="×") 
				{
					operand1=operand1 * operand2;
				}
				if (operator=="÷") 
				{
					if (operand2==0) 
					{
						operand1=0;
					} 
					else 
					{
						operand1 = operand1 / operand2;
					}
				}
				
				//結果をlabelに反映
				label.setText("" + operand1);
				eqflag = 1;
				//三つの項の計算の場合二つ目の演算子strを記憶する
				operator = str;
			}
			if (str == "=") 
			{
				operator = "";
				eqflag=1;
			}
			if (str == "²√x") 
			{
				operand1 = Math.sqrt(operand1);
				label.setText("" + operand1);
				//次そのまま計算するときに演算子を使うからoperatorを空にしておく
				operator = "";
				eqflag=1;
			}
			if (str == "x^2") 
			{
				operand1 = operand1 * operand1;
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if (str == "C") 
			{
				label.setText("0");
				operator = "";
				pointFlag=0;
			}
			if (str == "%") 
			{
				operand1 = operand1/100;
				label.setText("" + operand1);
				operator = "";
				eqflag = 1;
			}
			if(str=="x^3")
			{
				operand1 = operand1 * operand1*operand1;
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="x!")
			{
				int f=1;
				for(int i = 1; i <=operand1; i++)
				{
					f*=i;
				}
				operand1=f;
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="e^x")
			{
				operand1=Math.exp(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="cos")
			{
				operand1=Math.cos(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="sin")
			{
				operand1=Math.sin(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="tan")
			{
				operand1=Math.tan(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="ln")
			{
				operand1=Math.log(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;

			}
			if(str=="log₁₀x")
			{
				operand1=Math.log10(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="pie")
			{
				operand1=Math.PI;
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="e")
			{
				operand1=Math.E;
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="10^x")
			{
				operand1=Math.pow(10, operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="sinh")
			{
				operand1=Math.sinh(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="cosh")
			{
				operand1=Math.cosh(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;	
			}
			if(str=="tanh")
			{
				operand1=Math.tanh(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
			if(str=="³√x")
			{
				operand1=Math.cbrt(operand1);
				label.setText("" + operand1);
				operator= "";
				eqflag=1;
			}
		}
	}
}
