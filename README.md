# Eyes
简易动画_眨眼
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.awt.event.MouseEvent;  
import java.awt.event.MouseListener;
import java.util.*;
import java.lang.Integer;
import java.awt.Toolkit;

/**
 * 线程实现眼睛眨眼效果
 *通过sin函数模拟人眼的弧线
 * @author ThorinW
 */
class Eyes extends JFrame {

	
	public Eyes(String title){
		super();
		setTitle(title);
		Container contentPane = this.getContentPane();
		contentPane.setLayout(new BorderLayout());
		contentPane.add(new WindowTest(),BorderLayout.CENTER);
		setSize(200,200);
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
	}

    public static void main (String args[]) {

		Eyes eyes = new Eyes("眼睛");
		eyes.setVisible(true);
    }
	
	

}




class WindowTest extends JPanel{
	private int centerX = 200/2;//瞳孔圆心位置横坐标
	private int centerY = 200/2;//纵坐标
	private int len = 200;//眼睛横向长度
	private int hei = 50;//纵向高度
	private int htime = 0;//用于控制眨眼幅度
	private boolean flag = true;//作为眨眼幅度边界标记
	private int R = len/2;//眼睛半径
	private int r = hei/2;//瞳孔半径

	
	public WindowTest(){
		
		this.startRun(); 
	}
	
	
	
	public void paintComponent(Graphics g){
		super.paintComponent(g);		
		Graphics2D g2d = (Graphics2D)g;
		
		g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
		g2d.setColor(Color.darkGray);
		g2d.fillOval(0,0,200,200);//绘制灰色眼球
		g2d.setColor(Color.RED);
		g2d.fillOval(centerX-5,centerY-5,10,10);//瞳孔中间的小圆
		g2d.setColor(Color.WHITE);
		for(int i = 0;i < len;i++){
			//横坐标达到瞳孔半径范围以后，通过判断sin函数与圆弧位置高低绘制
			if((Math.sin(Math.PI*i/len)*htime) > Math.sqrt(r*r-(Math.abs(R-i)*Math.abs(R-i) )) && i >= centerX-r && i <= centerX+r){
				g2d.drawLine(centerX-len/2+i,
							centerY-(int)(Math.sin(Math.PI*i/len)*htime),
							centerX-len/2+i,
							centerY-(int)Math.sqrt(r*r-( Math.abs(R-i)*Math.abs(R-i) )  ) );
				g2d.drawLine(centerX-len/2+i,
							centerY+(int)Math.sqrt(r*r-(Math.abs(R-i)*Math.abs(R-i) ) ),
							centerX-len/2+i,
							centerY+(int)(Math.sin(Math.PI*i/len)*htime));	
			}
			//瞳孔半径范围外部只需画出白色范围即可
			else if(i < centerX-r || i > centerX+r){
				g2d.drawLine(centerX-len/2+i,
							centerY-(int)(Math.sin(Math.PI*i/len)*htime),
							centerX-len/2+i,
							centerY);
				g2d.drawLine(centerX-len/2+i,
							centerY,
							centerX-len/2+i,
							centerY+(int)(Math.sin(Math.PI*i/len)*htime));
			}
			
			
			
		}
		
		
		
	}
	public void startRun(){ 
		new Thread(){
			public void run(){
				while(true){
					if(flag){
						htime++;//幅度控制变量
						if(htime >= hei/2){
							flag = false;

						}
					}
					else {
						htime--;
						if(htime <= 0){
							flag = true;
						}
					}
					try{
						Thread.sleep(40);//通过线程实现效果
					}catch(InterruptedException e){
						e.printStackTrace();
					}
					repaint();
				}
			};
		}.start();
    }
	
	 
	
	
	
}
