---
layout: post
title: "Java Programming [Day 3]"
date: 2017-10-12 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---
        
# Java 예제 4일만에 뽀개기 - Day 3
        
    윈도우 - MFC
    자바 - JFC
    
    자바의 GUI(그래픽 사용자 인터페이스)의 종류
    
    1. AWT(Abstract Windows Toolkit) : 다른 플랫폼에서 제각각으로 나타난다
    2. SWING : 모든 플랫폼에서도 일관된 화면을 보여준다(충분히 GUI를 표현 가능함)
    
    - 컴포넌트 : 레이블, 버튼이나 텍스트 필드와 같은 GUI를 작성하는 기본적인 빌딩 블록
    - J가 붙은 이름 : 스윙에서 만드는 컴포넌트
    
    - 컨테이너 : 다른 컴포넌트들을 내부에 넣을 수 있는 컴포넌트
    
        1. 최상위 컨테이너: 다른 컨테이너 안에 포함될 수 없는 컨테이너(JFrame, JDialog, JApplet을 먼저 만들어준다)
        2. 일반 컨테이너: 다른 컨테이너 안에 포함될 수 있는 컨테이너
    
    일반적으로 JFrame을 만들고 JPanel을 만든다.
    JPanel에 컴포넌트를 모두 작성, JFrame에 통째로 집어넣는다
    
    - 프레임의 속성을 설정하는 중요한 메소드
    
        1. setLocation(x, y) / setBounds(x, y, width, height) / setSize(width, height) : 프레임의 위치와 크기 설정
        2. setIconImage(IconImage) : 윈도우 시스템에 타이틀 바, 태스크 스위처에 표시할 아이콘을 알려줌
        3. setTitle(String title) : 타이틀 바의 제목을 변경한다.
        4. setResizable(boolean) : 사용자가 크기를 조절할 수 있는지를 설정한다.

    - 프레임 : 메뉴를 붙일 수 있는 윈도우
    
        1. JFrame() : 타이틀이 없는 새로운 프레임
        2. JFrame(String title) : 지정된 타이틀의 새로운 프레임 
        3. add(Component c) : 지정된 컴포넌트를 프레임에 추가
        4. pack() : 프레임을 크기를 추가된 컴포넌트들의 크기에 맞도록 조절
        5. remove(Component c) : 지정된 컴포넌트를 프레임에서 제거
        6. setDefaultCloseOperation(일반적으로 JFrame.EXIT_ON_CLOSE) : 사용자가 프레임을 닫을 때 수행되는 동작을 설정함. 
        7. setIconImage(Icon image) : 프레임이 최소화되었을때의 아이콘 지정
        8. setLayout(LayoutManager layout) : 프레임위에 놓이는 컴포넌트들을 배치하는 배치관리자 지정 -> 디폴트는 BorderLayout / 일반적으로 new FlowLayout()
        9. setLocation(int x, int y) : 프레임의 x좌표와 y좌표를 지정
           setLocationRelativeTo(null) // 중앙으로 위치
        10. setResizable(boolean value) : 프레임의 크기 변경 허용 여부
        11. setSize(int width, int height) : 프레임의 크기 설정
        12. setMenuBar(JMenuBar menu) : 현재 프레임에 메뉴바를 붙임

    - 패널 : 컴포넌트들을 포함하고 있도록 설계된 컨테이너 중 하나
    
        1. JPanel() : 새로운 패널을 생성
        2. JPanel(boolean isDoubleBuffered) : 만약 매개변수가 참이면 더블 버퍼링을 사용
        3. JPanel(LayoutManager layout) : 지정된 배치관리자를 사용하는 패널을 생성
        4. add(Component c) : 지정된 컴포넌트를 패널에 추가
        5. remove(Component c) : 지정된 컴포넌트를 패널에서 제거
        6. setLayout(LayoutManager layout) : 배치관리자를 지정, 디폴트는 FlowLayout
        7. setLocation(int x, int y) : 패널의 위치 지정
        8. setSize(int width, int height) : 패널의 크기 지정
        9. setToolTipText(String next) : 사용자가 마우스를 패널의 빈 곳에 올려놓으면 툴팁을 표시

    - 레이블 : 편집이 불가능한 텍스트를 표시하기 위한 컴포넌트
    
        1. JLabel() : 새로운 레이블을 생성
        2. JLabel(String text) : 지정된 텍스트를 표시하는 레이블을 생성
        3. getText() : 레이블의 텍스트를 반환
        4. setText(String text) : 레이블의 텍스트를 설정
        5. setToolTipText(String text) : 사용자가 마우스를 레이블 위에 올려놓으면 툴팁으로 표시
        6. setVisible(boolean value) : 레이블을 보이게 하거나 감춤

    - 버튼 : 사용자가 클릭했을 경우, 이벤트를 발생하여 원하는 동작을 하게 하는데 이용된다.
    
        1. Button() : 레이블이 없는 버튼을 생성
        2. Button(String label) : 지정된 레이블의 버튼을 생성
        3. getText() : 버튼의 현재 텍스트를 반환
        4. setText(String text) : 버튼의 텍스트를 설정
        5. doClick() : 사용자가 버튼을 누른 것처럼 이벤트를 발생
        6. setBorderPainted(boolean value) : 버튼의 경계를 나타내거나 감춤
        7. setContentAreaFilled(boolean value) : 버튼의 배경을 채울 것인지를 지정
        8. setEnabled(boolean value) : 버튼을 활성화하거나 비활성화
        9. setRolloverEnabled(boolean value) : 마우스가 버튼 위에 있으면 경계를 진하게 하는 롤오버 효과를 설정
        10. setToolTipText(String text) : 사용자가 마우스를 버튼 위에 올려놓으면 툴팁을 표시
        11. setVisible(boolean value) : 버튼을 보이게 하거나 감춤

    - 텍스트 필드
    
        1. JTextField() : TextField를 생성
        2. JTextField(int columns) : 지정된 칸 수를 가지고 있는 TextField를 생성
        3. JTextField(String text) : 지정된 문자열로 초기화된 TextField를 생성
        4. setText(String text) : 지정된 문자열을 텍스트 필드에 씀
        5. getText() : 텍스트 필드에 입력된 문자열을 반환
        6. setEditable(boolean) : 사용자가 텍스르틀 입력할 수 있는지 없는지를 설정하고 반환
           boolean isEditable() : 사용자가 텍스르틀 입력할 수 있는지 없는지를 설정하고 반환

---

    import javax.swing.*;
    import java.awt.*; // layout 관련 class
    
    class MyFrame extends JFrame // JFrame class 상속
    {
        public MyFrame()
        {
    //      1. 컴포넌트 설정
    //		super(); // 생략 가능(자동 호출), 타이틀 제목
            setSize(500, 200);
                    // 이 라인은 무조 건 있어야함, 없으면 프로세스가 종료되지 않음
                    // 없을 경우 x를 눌렀을 때 화면은 사라져도 백그라운드에서 계속 돌아간다
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
            setTitle("My Frame Test");
            setLocation(100, 200); // default는 0, 0
    
    //      2. Layout 설정
                    // component 배치 설정, default = BorderLayout, FlowLayout() -> 왼쪽에서 오른쪽으로 하나씩 들어감
                    // FlowLayout이란 컴포넌트를 물이 흐르듯 순차적으로 배치하는 방법
                    // 만약 배치관리자를 지정하지 않고 버튼을 프레임에 추가하면 버튼이 전체 화면을 차지한다
                    // 왜냐하면 디폴트 값으로 BorderLayout(border = 국경)이 들어가기 때문에
            setLayout(new FlowLayout()); 
    
    //      3. 버튼 설정		
            JButton button = new JButton("Button"); // button 만들기
            add(button);
            
                    // 이 라인 역시나 무조건 있어야함
                    // 없으면 프레임은 만들어져도 화면에 나타나지 않는다
            setVisible(true); 
        }	
    }
    
    public class MyFrameTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }

---

    import javax.swing.*;
    import java.awt.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            super("Test"); // 타이틀 바에 문자열을 줄 수 있음
            Toolkit kit = Toolkit.getDefaultToolkit(); // graphic 관련 자원 정보 제공
            Dimension screenSize = kit.getScreenSize(); // 화면 크기 반납(width, height);
            System.out.println(screenSize.width + ", " + screenSize.height); // 가로, 세로의 픽셀 값 출력
            
            setSize(300, 200);
            setLocation(screenSize.width / 2, screenSize.height / 2); // 중간 위치에 윈도우를 위치 시켜라
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    //		setTitle("My Frame"); // 타이틀 바에 문자열
            
            Image img = kit.getImage("mystery.jpg"); // 이미지 첨부
            setIconImage(img);
            
            setLayout(new FlowLayout());
            JButton button = new JButton("Button");
            this.add(button);
            
            setResizable(false);
            setVisible(true);
            
        }
    }
    
    public class MyFrameTest2 {
    
        public static void main(String[] args) {
            MyFrame fr = new MyFrame();
        }
    }

---

    import javax.swing.*;
    import java.awt.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setSize(300, 20);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("My Frame");
            
            JPanel panel = new JPanel();
            JLabel label = new JLabel("Hello World");
            JButton button = new JButton("Button");
            
    //		button.setBorderPainted(false); // 외곽선 모양
    //		button.setContentAreaFilled(false); // 컴포넌트의 그라데이션
            button.setRolloverEnabled(true); // 마우스를 올려놓으면 버튼을 누르는 형태
            button.setToolTipText("Button test"); // 마우스를 올려놓으면 버튼이 무슨 역할을 하는지 보여줌
            
            //앞으로 다음과 같은 방식으로 진행한다
            panel.add(label); // panel에 component 추가
            panel.add(button);
            add(panel); // frame에 panel 추가
            setVisible(true);
            System.out.println(label.getText());
        }
    }
    
    public class MyFrameTest3 {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
    
        }
    
    }

---

    import javax.swing.*;
    import java.awt.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame() {
        setSize(500, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setTitle("My Frame");
        setLocation(100, 200);
        
        JPanel panel = new JPanel();
        JLabel label = new JLabel("Hellow World");
        
        JButton button1 = new JButton("Left");
        JButton button2 = new JButton("Center");
        JButton button3 = new JButton("Right");
        
        button3.setBorderPainted(false);
        button3.setContentAreaFilled(false);
        button3.setRolloverEnabled(false);
        button1.setToolTipText("Button test");
        button2.setToolTipText("Button up");
        
        panel.add(label);
        panel.add(button1);
        panel.add(button2);
        panel.add(button3);
        
        JTextField t1 = new JTextField(10);
        JTextField t2 = new JTextField(10);
        t2.setEditable(false);
        t2.setText("Not Editable");
        
        panel.add(t1);
        panel.add(t2);
        add(panel);
        setVisible(true);
        System.out.println(label.getText());
        }
    }
    public class MyFrameTest4 {
        public static void main(String[] args)
        {
            MyFrame f = new MyFrame();
        }
    }

---

    import javax.swing.*;
    import java.awt.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setSize(300, 150);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setVisible(true);
            
            JPanel panel = new JPanel();
            JPanel panelA = new JPanel();
            JPanel panelB = new JPanel();
            
            // panelA에 Label을 입력
            JLabel label = new JLabel("Welcome to Java Pizza. Select Pizza");
            panelA.add(label);
            
            JButton b1 = new JButton("Combo");
            JButton b2 = new JButton("Potato");
            JButton b3 = new JButton("Bulgogi");
            
            // panelB에 Button을 입력
            panelB.add(b1);
            panelB.add(b2);
            panelB.add(b3);
            
            // panel에 panelA와 panelB를 입력
            panel.add(panelA);
            panel.add(panelB);
            
            // panel을 Frame에 입력
            panel.setBackground(Color.BLUE);
            panelA.setBackground(Color.GREEN);
            panelB.setBackground(new Color(255, 100, 100)); // r, g, b
            add(panel);
        }
    }
    public class LABTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
    
        }
    
    }

---
    
    배치 관리자 종류
        1. FlowLayout(왼쪽에서 오른쪽으로 순서대로)
        2. GridLayout(격자로 나뉘어짐)
        3. GridBagLayout(GridLayout의 확장판)
        4. BorderLayout(5개의 영역 - "North", "South", "West", "East", "Center")
        5. BoxLayout(FlowLayout과 같다, x축에서 y축으로 넣을 수도 있다)
        6. CardLayout(이벤트 핸들러를 배우고선)
    
    배치 관리자 설정
        1. 생성자를 이용하는 방법
           JPanel panel = new JPanel(new BorderLayout()); // 한 번에 만들기 때문에 이것이 가장 편하다
        2. setLayout() 메소드 이용
           JPanel panel = new JPanel(); // 객체 생성 후 
           panel.setLayout(new FlowLayout()); // 배치 관리자 설정
    
    크기와 정렬 힌트
    setMinimuSize(), setPreferredSize(), setMaximumSize() // 별로 사용하지 않으며 지정하기가 어렵다
    ex) button.setMaximumSize(new Dimension(300, 200)) // 버튼의 최대 크기 힌트
    
    
    1. FlowLayout
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setTitle("FlowLayout Test");
            setSize(500, 200);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            
    //      JPanel panel = new JPanel();
    //      panel.setLayout(new FlowLayout(FLowLayout.LEADING, 10, 20));
            JPanel panel = new JPanel(new FlowLayout(FlowLayout.LEADING, 10, 20)); // x간격을 10픽셀, y간격을 20픽셀
            // FlowLaytout.LEADING(왼쪽 정력), FlowLayout.CENTER(가운데 정렬), FlowLayout.TRAILING(오른쪽 정렬)
            panel.add(new JButton("Button1"));
            panel.add(new JButton("Button2"));
            panel.add(new JButton("Button3"));
            panel.add(new JButton("Button4"));
            panel.add(new JButton("Long Button5"));
            add(panel);
            panel.applyComponentOrientation(ComponentOrientation.RIGHT_TO_LEFT); // 방향을 설정, 오른쪽에서 왼쪽으로 정렬해줌
            pack(); // frame 크기 조절, component 사이즈에 맞도록 패널 사이즈 조절(압축)
            setVisible(true);
        }
    }
    
    public class FlowTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }


    2. BorderLayout
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setTitle("BorderLayout Test");
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLayout(new BorderLayout(5, 5)); // JFrame은 default로 BorderLayout
                                               // 컴포넌트의 x축 5픽셀, y축 5픽셀 설정
            setLocationRelativeTo(null);
            setSize(500, 400);
            
            JButton button = new JButton("Center");
            add(button, BorderLayout.CENTER); // add할 때 위치 지정
    //      button.setBounds(10, 20, 50, 60);
            add(new JButton("West"), BorderLayout.WEST);
            add(new JButton("East"), BorderLayout.EAST);
            add(new JButton("North"), BorderLayout.NORTH);
            add(new JButton("South"), BorderLayout.SOUTH);
            
            pack();
            setVisible(true);
        }
    }
    public class BorderTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }


    3. GridLayout 예제
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setTitle("GridLayout Test");
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLayout(new GridLayout(2, 3, 5, 5)); // 행과 열의 개수, 수평과 수직 간격
            setLocationRelativeTo(null);
            setSize(500, 300);
            
            JButton b1 = new JButton("Button1");
            add(b1);
            add(new JButton("Button2"));
            add(new JButton("Button3"));
            add(new JButton("B4"));
            add(new JButton("Long Button5"));
            
    //		pack();
            setVisible(true);
        }
    }
    public class GridTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }


    4. BoxLayout 예제
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setTitle("BoxLayout Test");
            setSize(300, 500);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            JPanel panel = new JPanel();
            
            panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));
            
            makeButton(panel, "Button1");
            makeButton(panel, "Button2");
            makeButton(panel, "BUtton3");
            makeButton(panel, "B4");
            makeButton(panel, "Long Button5");
            
            add(panel);
            pack();
            setVisible(true);
            
        }
        
        private void makeButton(JPanel p, String text) // 함수를 만들어서 버튼 설정하기
        {
            JButton button = new JButton(text);
            button.setAlignmentX(Component.CENTER_ALIGNMENT); 
                    // LEFT_ALIGNMENT, CENTER_ALIGNMENT, RIGHT_ALIGNMENT
            button.setMaximumSize(new Dimension(100, 40));
            p.add(button);
        }
    }
    public class BoxTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }


    5. AbsoluteLayout 예제
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame
    {
        private JButton b1, b2, b3;
        
        public MyFrame()
        {
            setTitle("Absolute Test");
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setSize(400, 500);
            
            JPanel p = new JPanel();
            p.setLayout(null); // 배치 관리자 제거
            b1 = new JButton("Button #1");
            b2 = new JButton("Button #2");
            b3 = new JButton("Button #3");
            p.add(b1);
            p.add(b2);
            p.add(b3);
            b1.setBounds(20, 5, 95, 30); // 절대 위치 지정(x, y, w, h)
            b2.setBounds(55, 45, 105, 70);
            b3.setBounds(180, 15, 105, 90);
            add(p);
            setVisible(true);
        }
    }
    public class AbsoluteTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
    
        }
    
    }

---

    import java.awt.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setTitle("Interest Calculator");
            setSize(400, 300);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLayout(new GridLayout(4, 1));
            JPanel p1, p2, p3, p4;
            
            
            p1 = new JPanel();
            p2 = new JPanel();
            p3 = new JPanel();
            p4 = new JPanel();
            
            JLabel l1 = new JLabel("Enter Principal : ");
            JTextField f1 = new JTextField(10);
            JLabel l2 = new JLabel("Enter Interest Rate : ");
            JTextField f2 = new JTextField(10);
            JButton b = new JButton("Calculate");
            JTextField f3 = new JTextField(20);
            
            p1.add(l1);
            p1.add(f1);
            p2.add(l2);
            p2.add(f2);
            p3.add(b);
            p4.add(f3);
            add(p1); add(p2); add(p3); add(p4);
            
    //		pack();
            setVisible(true);
        }
    }
    public class MyFrameTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }
    
    자바 그래픽의 두 가지 방법
    1. AWT
    2. Java 2D
    
    그래픽 좌표계
    - y축이 증가할 수록 아래로 내려감

---

    // Graphics
    // 만약 프레임이 백그라운드에 가있더라도 
    // paintComponet가 컴포넌트들을 원상태로 복귀
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
    // JComponent 클래스의 paintComponent를 재정의
        public void paintComponent(Graphics g) // Graphics에 관한 모든 것
                                               // Graphics 객체를 매개변수로(반드시)
        {
            super.paintComponent(g); // 반드시 추가
            g.setColor(Color.green);
            g.drawString("Hello, Java Programmers!!", 10, 10);
            g.drawLine(10, 20, 110, 20);
            g.drawRect(10, 30, 100, 100); // x, y, w, h
        }
    }
    
    public class MyFrame extends JFrame
    {
        public MyFrame()
        {
            setTitle("My Frame");
            setSize(400, 300);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            MyPanel p = new MyPanel();
            add(p);
            setVisible(true);
        }
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }

---

    import java.awt.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
        public void paintComponent(Graphics g)
        {
            super.paintComponent(g); // 무조건 우선 순위 
                                     // 필요한 이유 : 백그라운드에서 포그라운드로 
                                             // 나올때라던지 가려져있다가 나타날때 자동으로 
                                             // 호출되어 다시 그려준다
            g.setColor(Color.BLUE);
            g.drawRect(50, 50, 50, 50);
            g.fillRect(150, 50, 50, 50);
            
            g.draw3DRect(250, 50, 50, 50, true); // 3D같은 모양이 되게 음영을 넣어줌
            g.fill3DRect(350, 50, 50, 50, true);
            
            g.drawRoundRect(450, 50, 50, 50, 10, 10);
            g.fillRoundRect(550, 50, 50, 50, 10, 10);
        }
    }
    
    class MyFrame extends JFrame
    {
        public MyFrame()
        {
            super("Basic Shape Drawing");
            setSize(700, 400);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setVisible(true);
            MyPanel p = new MyPanel();
            add(p);
        }
    }
    
    public class BasicShapeTest 
    {
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }

    // 1단계 웃는 얼굴
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
        public void paintComponent(Graphics g)
        {
            super.paintComponent(g);
            setLocation(40, 10);
            g.setColor(Color.YELLOW);
            g.fillOval(20, 40, 200, 200);
            g.setColor(Color.BLACK);
            g.drawArc(60, 80, 50, 50, 180, -180); // -가 붙으면 시계 방향으로, 왼쪽 눈
            g.drawArc(150, 80, 50, 50, 0, 180); // +이면 반시계 방향으로, 오른쪽 눈
            g.drawArc(70, 130, 100, 70, 0, -180); // 입
        }
    }
    
    public class SunManFace extends JFrame {
    
        public SunManFace()
        {
            setSize(350, 350);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("Sun Man Face");
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            SunManFace f = new SunManFace();
        }
    
    }

    // 2단계 찡그린 얼굴
    /*
        Event handling 순서
        1) 컴포넌트가 어떤 event를 발생하는가(JButton -> ActionEvent 발생)
        2) event에 따른 Listener interface를 구현하는 클래스 작성
           (ActionEvent -> ActionListener interface)
        3) Listener 객체 생성
        4) 컴포넌트에 Listener 객체 등록
    */
    // 지금까지는 main에서 끝났지만 이제는 계속해서 대기한다(버튼을 누를 때마다 실행)
    // -> Event driven 프로그래밍
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
        JButton button;
    //  Color color = new Color(255, 255, 0);
        Color color = Color.YELLOW;
        MyListener listener = new MyListener();
        boolean smile = true;
        
        public MyPanel()
        {
            button = new JButton("Change Button");
            add(button);
            button.addActionListener(listener); // button에 ActionListener 등록
            // 만약 button이 눌려지면 자동으로 actionPerformed가 실행된다
        }
        
        public void paintComponent(Graphics g)
        {
            super.paintComponent(g);
            setLocation(40, 10);
            g.setColor(color);
            g.fillOval(20, 40, 200, 200);
            g.setColor(Color.BLACK);
            if(smile)
            {
                g.drawArc(60, 80, 50, 50, 0, 180); // -가 붙으면 시계 방향으로, 왼쪽 눈
                g.drawArc(150, 80, 50, 50, 0, 180); // +이면 반시계 방향으로, 오른쪽 눈
                g.drawArc(70, 130, 100, 70, 0, -180); // 입
            }
            else
            {
                g.drawArc(60, 80, 50, 50, 0, -180); 
                g.drawArc(150, 80, 50, 50, 0, -180); 
                g.drawArc(70, 130, 100, 70, 0, 180); 
            }
        }
        
        private class MyListener implements ActionListener // 내부 클래스로 작성
        {
            public void actionPerformed(ActionEvent e) // 시그니처 틀리지 않기
            {
                smile = !smile;
                repaint(); // paintComponent 호출
            }
        }
    }
    
    public class SunManFace extends JFrame {
    
        public SunManFace()
        {
            setSize(350, 350);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("Sun Man Face");
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            SunManFace f = new SunManFace();
        }
    
    }

    // 3단계 얼굴 색 바꾸기
    // Color(Red, Green, Blue) 각각 0~255까지 색상의 수
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
        JButton button;
    //  Color color = new Color(255, 255, 0);
        Color color = Color.YELLOW;
        MyListener listener = new MyListener();
        boolean smile = true;
        
        public MyPanel()
        {
            button = new JButton("Change Button");
            add(button);
            button.addActionListener(listener); 
        }
        
        public void paintComponent(Graphics g)
        {
            super.paintComponent(g);
            setLocation(40, 10);
            g.setColor(color);
            g.fillOval(20, 40, 200, 200);
            g.setColor(Color.BLACK);
            if(smile)
            {
                g.drawArc(60, 80, 50, 50, 0, 180); 
                g.drawArc(150, 80, 50, 50, 0, 180); 
                g.drawArc(70, 130, 100, 70, 0, -180);
            }
            else
            {
                g.drawArc(60, 80, 50, 50, 0, -180); 
                g.drawArc(150, 80, 50, 50, 0, -180); 
                g.drawArc(70, 130, 100, 70, 0, 180); 
            }
        }
        
        private class MyListener implements ActionListener
        {
            public void actionPerformed(ActionEvent e) 
            {
                // ActionListener interface를 구현하는 Listener 클래스 작성
                color = new Color((int)(Math.random() * 255),
                        (int)(Math.random() * 255),
                        (int)(Math.random() * 255));
                repaint(); // paintComponent 호출
            }
        }
    }
    
    public class SunManFace extends JFrame {
    
        public SunManFace()
        {
            setSize(350, 350);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("Sun Man Face");
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            SunManFace f = new SunManFace();
        }
    
    }

    // 4단계 얼굴 색 선택 바꾸기
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
        JButton button;
    //  Color color = new Color(255, 255, 0);
        Color color = Color.YELLOW;
        MyListener listener = new MyListener();
        boolean smile = true;
        
        public MyPanel()
        {
            button = new JButton("Change Button");
            add(button);
            button.addActionListener(listener); 
        }
        
        public void paintComponent(Graphics g)
        {
            super.paintComponent(g);
            setLocation(40, 10);
            g.setColor(color);
            g.fillOval(20, 40, 200, 200);
            g.setColor(Color.BLACK);
            if(smile)
            {
                g.drawArc(60, 80, 50, 50, 0, 180); 
                g.drawArc(150, 80, 50, 50, 0, 180); 
                g.drawArc(70, 130, 100, 70, 0, -180);
            }
            else
            {
                g.drawArc(60, 80, 50, 50, 0, -180); 
                g.drawArc(150, 80, 50, 50, 0, -180); 
                g.drawArc(70, 130, 100, 70, 0, 180); 
            }
        }
        
    
        private class MyListener implements ActionListener
        {
            public void actionPerformed(ActionEvent e) 
            {
                // ActionListener interface를 구현하는 Listener 클래스 작성
                color = JColorChooser.showDialog(MyPanel.this, "Choose a color", color);
                // parent component, title, initial color
                repaint(); // paintComponent 호출
            }
        }
    
    
    }
    
    public class SunManFace extends JFrame {
    
        public SunManFace()
        {
            setSize(350, 350);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("Sun Man Face");
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            SunManFace f = new SunManFace();
        }
    
    }

    // 폰트 바꾸기
    
    import java.awt.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel
    {
        Font f;
        
        public MyPanel()
        {
            String[] fontList; // 컴퓨터에 설치된 폰트 리스트로 출력하는 방법
            GraphicsEnvironment ge = GraphicsEnvironment.getLocalGraphicsEnvironment();
            fontList = ge.getAvailableFontFamilyNames();
            for(int i = 0; i < fontList.length; i++)
                System.out.println(fontList[i]);
        }
        
        public void paintComponent(Graphics g)
        {
            super.paintComponent(g);
            f = new Font("Agency FB", Font.BOLD, 60); // Font 객체 생성
            // 이름, 스타일, 크기(1 포인트 = 1/72인치)
            g.setFont(f); // Font 설정
            g.drawString("Ji Myung Hwa", 100, 50);
            
            f = new Font("Serif", Font.BOLD | Font.ITALIC | Font.PLAIN, 50);
            g.setFont(f);
            g.drawString("Ji Myung Hwa", 100, 150);
        }
    }
    
    public class FontTest extends JFrame {
    
        public FontTest()
        {
            setSize(500, 500);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("Font Test");
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            FontTest f = new FontTest();
        }
    
    }

---

    import java.awt.*;
    import javax.swing.*;
    import java.awt.geom.*; // 도형틀의 클래스
    
    class MyPanel extends JPanel
    {
        public void paintComponent(Graphics g)
        {
            Shape s; // Shape interface 참조 변수로 모든 도형 참조 가능
                     // geom 패키지로 모든 클래스는 Shape interface를 구현하기 때문
            
            super.paintComponent(g);
            Graphics2D g2 = (Graphics2D)g;
            
            g2.setColor(Color.black);
            g2.setStroke(new BasicStroke(5)); // 두께 설정
            s = new Rectangle2D.Double(10, 10, 50, 50);
            g2.draw(s);
            s = new RoundRectangle2D.Double(110, 10, 100, 100, 20, 20);
            g2.draw(s);
            
            g2.setFont(new Font("Courier", Font.BOLD, 30));
            g2.drawString("Text antialiasing", 50, 150);
    // Graphics는 도형의 객체 생성없이 출력
    // Graphics2D는 도형의 객체 생성 후 draw() 메소드 사용하여 출력
        }
    }
    
    public class MoreShapes extends JFrame {
    
        public MoreShapes()
        {
            setSize(500, 500);
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setTitle("Java 2D Shapes");
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            new MoreShapes();
        }
    
    }

---

    import java.awt.*;
    import javax.swing.*;
    import java.awt.geom.*;
    
    class MyPanel extends JPanel {
        public void paintComponent(Graphics g) {
            Shape s;
            super.paintComponent(g);
            Graphics2D g2 = (Graphics2D)g;
            
            g2.setColor(Color.BLACK);
            g2.setStroke(new BasicStroke(5));
            
            s = new Rectangle2D.Double(10, 10, 70, 80);
            g2.draw(s);
            
            s = new RoundRectangle2D.Double(110, 10, 70, 80, 20, 20);
            g2.draw(s);
            
            s = new Ellipse2D.Double(210, 10, 80, 80);
            g2.draw(s);
            
            s = new Arc2D.Double(310, 10, 80, 80, 0, 270, Arc2D.OPEN);
            g2.draw(s);
            
            s = new Arc2D.Double(410, 10, 80, 80, 0, 270, Arc2D.CHORD);
            g2.draw(s);
            
            s = new Arc2D.Double(510, 10, 80, 80, 0, 270, Arc2D.PIE);
            g2.draw(s);		
        }
    }
    
    public class practice extends JFrame {
        public practice() {
            setTitle("Java 2D shapes");
            setSize(700, 300);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            add(new MyPanel());
            setVisible(true);
        }
        
        public static void main(String[] args) {
            practice p = new practice();
        }
    }

---

    - 이벤트 처리
    - 버튼이 눌렸을때 어느 버튼이 눌렸는지 확인해주는 함수
    getSource() // ActionEvent를 발생시키는 어떤 컴포넌트인지 판별해줌
                // Object 타입으로 반환하므로, 이것을 필요한 타입으로 형변환하여서 사용하면 된다
    
    - 이벤트 처리기
        1. 독립적인 클래스(신경 x)
        2. 내부 클래스(가장 많이 사용) -> 15장
        3. 프레임 클래스(신경 x)
        4. 무명 클래스(그 다음 사용) -> 핸드폰 어플 만들 때 많이 사용

    ex) 내부 클래스 - 다른 클래스 안에 위치하는 클래스. 외부 클래스의 멤버 변수들을 자유롭게 사용할 수 있다.
    
    class MyListener implements ActionListener {
        public void actionPerformed(ActionEvent e) {
            JButton button = (JButton)e.getSource(); // ActionEvent가 발생한 객체 반환
            button.setText("Button Pressed");
            // label.setText("Button is pressed");
            // label은 MyFrame class의 private mameber이기 때문에 접근이 어려움
        }
    }
    
    // 단점 -> 레이블이 독립적인 클래스 ActionListener에서 접근하기 위해선
               setter함수를 사용해야함
            -> 내부 클래스를 사용하자
    
    
    // Event handler를 inner class로 작성하는 경우
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame { 
        private JButton button;
        private JLabel label;
        private int count = 0; // 클릭 횟수 count
        
        public MyFrame() {
            setSize(500, 200);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            setLayout(new FlowLayout(FlowLayout.CENTER));
            setTitle("Event Example");
            
            JPanel panel = new JPanel();
            button = new JButton("Press Button");
            label = new JLabel("Button is not pressed yet");
            button.addActionListener(new MyListener());
            panel.add(button);
            panel.add(label);
            add(panel);
    //		pack();
            setVisible(true);
        }
        // 내부 클래스로 작성
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                if(e.getSource() == button) // event 발생 객체의 이름 리턴
                    label.setText("Button is pressed : " + ++count);
            }
        }
    }
    
    public class ActionEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    }

---

    무명 클래스
    
    // Event handler를 anonymous class(무명 클래스)로 작성하는 경우
    // 컴포넌트가 몇 개 되지 않은 경우에 한하여 사용함 -> 거의 사용되지 않음
    // 간단하지만 Button이 여러개인 경우 button마다 작성해야 함
    // button마다 ActionListener를 넣어줌
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame { 
        private JButton button;
        private JLabel label;
        private int count = 0; // 클릭 횟수 count
        
        public MyFrame() {
            setSize(500, 200);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            setLayout(new FlowLayout(FlowLayout.CENTER));
            setTitle("Event Example");
            
            JPanel panel = new JPanel();
            button = new JButton("Press Button");
            label = new JLabel("Button is not pressed yet");
            button.addActionListener(new ActionListener() { // ActionListener가 매개변수 안에 들어감
                public void actionPerformed(ActionEvent e) { // 이름이 없다 -> 다른 곳에서 쓸 수 없고 버튼에서만 사용 -> 무명 클래스
                    if(e.getSource() == button)
                        label.setText("Button is pressed : " + ++count);
                }
            });	// 여기까지 매개변수이고, 매개변수 안에 클래스에 대한 정의가 있다
            // 단점 -> 만약 버튼이 5개이면 위의 시그니처로 버튼마다 작성해주어야 함	
            
            panel.add(button);
            panel.add(label);
            add(panel);
    //		pack();
            setVisible(true);
        }
    }
    
    public class ActionEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    }

---

    모든 컴포넌트가 지원하는 이벤트
    
        1. Component - 컴포넌트의 크기나 위치가 변경되었을 경우 발생
        2. Focus - 키보드 입력을 받을 수 있는 상태가 되었을 때, 혹은 그 반대의 경우 발생
        3. Container - 컴포넌트가 컨테이너에 추가되거나 삭제될 때 발생
        4. Key - 사용자가 키를 눌렀을 때 키보드 포커스를 가지고 있는 객체에서 발생
        5. Mouse - 마우스 버튼이 클릭되었을 때, 또는 마우스가 객체의 영역으로 들어오거나 나갈 때 발생
        6. MouseMotion - 마우스가 움직였을 때 발생
        7. MouseWheel - 컴포넌트 위에서 마우스 휠을 움직이는 경우 발생
        8. Window - 윈도우에 어떤 변화가 있을 때 발생(열림, 닫힘, 아이콘화 등)
    
    일부 컴포넌트가 지원하는 이벤트
    
        1. Action - 사용자가 어떤 동작을 하는 경우에 발생
        2. Caret - 텍스트 삽입점이 이동하거나 텍스트 선택이 변경되었을 경우 발생
        3. Change - 일반적으로 객체의 상태가 변경되었을 경우 발생
        4. Document - 문서의 상태가 변경되는 경우 발생
        5. Item - 선택 가능한 컴포넌트에서 사용자가 선택을 하였을 때 발생
        6. ListSelection - 리스트나 테이블에서 선택 부분이 변경되었을 경우에 발생

    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame { 
        private JButton b1, b2, b3;
        private JTextField t;
        private JPanel panel;
        MyListener listener = new MyListener();
        
        public MyFrame() {
            setSize(500, 200);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            setTitle("Event Example");
            
            panel = new JPanel();
            
            b1 = new JButton("RED");
            b1.addActionListener(listener);
            
            b2 = new JButton("Orange");
            b2.addActionListener(listener);
            
            b3 = new JButton("Yellow");
            b3.addActionListener(listener);
            
            t = new JTextField(10);
            
            panel.add(b1);
            panel.add(b2);
            panel.add(b3);
            panel.add(t);
            add(panel);
            setVisible(true);
        }
        
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                // 여러개의 컴포넌트 중 어느 컴포넌트에서 이벤트가 발생하였는지
                // 알기위해서 getSource 메소드를 작성한다
                if(e.getSource() == b1) { // 객체 이름 반환
                    t.setText("b1 clicked");
                    panel.setBackground(Color.RED);
                }
                
                if(e.getSource() == b2) {
                    t.setText("b2 clicked");
                    panel.setBackground(Color.ORANGE);
                }
                
                if(e.getSource() == b3) {
                    t.setText("b3 clicked");;
                    panel.setBackground(Color.YELLOW);
                }
            }
        }
    }
    
    public class ActionEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    }

    // 무지개 색깔 표현
    
    import java.awt.*;
    import javax.swing.*;
    import java.awt.event.*;
    
    class MyFrame extends JFrame {
        private JButton b1, b2, b3, b4, b5, b6, b7;
        private JLabel label;
        private JPanel panel;
        private int redCount = 0;
        private int orangeCount = 0;
        private int yellowCount = 0;
        private int greenCount = 0;
        private int blueCount = 0;
        private int indigoCount = 0;
        private int puppleCount = 0;
        MyListener listener = new MyListener();
        
        public MyFrame() {
            setSize(700, 400);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            setTitle("Event example");
            
            panel = new JPanel();
            
            b1 = new JButton("Red"); b1.addActionListener(listener);
            b2 = new JButton("Orange"); b2.addActionListener(listener);
            b3 = new JButton("Yellow"); b3.addActionListener(listener);
            b4 = new JButton("Green"); b4.addActionListener(listener);
            b5 = new JButton("Blue"); b5.addActionListener(listener);
            b6 = new JButton("Indigo"); b6.addActionListener(listener);
            b7 = new JButton("Pupple"); b7.addActionListener(listener);
            
            label = new JLabel("Button is not pressed yet");
            
            panel.add(b1);panel.add(b2);panel.add(b3);panel.add(b4);panel.add(b5);panel.add(b6);panel.add(b7);
            panel.add(label);
            add(panel);
            setVisible(true);
        }
        
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                if(e.getSource() == b1) {
                    label.setText("Red On" + ++redCount);
                    panel.setBackground(Color.RED);
                }
                if(e.getSource() == b2) {
                    label.setText("Orange On" + ++orangeCount);
                    panel.setBackground(Color.ORANGE);
                }
                if(e.getSource() == b3) {
                    label.setText("Yellow On" + ++yellowCount);
                    panel.setBackground(Color.YELLOW);
                }
                if(e.getSource() == b4) {
                    label.setText("Green On" + ++greenCount);
                    panel.setBackground(Color.GREEN);
                }
                if(e.getSource() == b5) {
                    label.setText("Blue On" + ++blueCount);
                    panel.setBackground(Color.BLUE);
                }
                if(e.getSource() == b6) {
                    label.setText("Indigo On" + ++indigoCount);
                    panel.setBackground(new Color(0, 0, 128));
                }
                if(e.getSource() == b7) {
                    label.setText("Pupple On" + ++puppleCount);
                    panel.setBackground(new Color(128, 0, 255));
                }
            }
        }
    }
    
    public class ActionEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }

---

    -KeyEvent
    -KeyListener 인터페이스
    
    // Keyboard는 KeyEvent 발생
    // KeyListener를 구현한다
    
    import javax.swing.*;
    import java.awt.event.*;
    
    class MyFrame extends JFrame {
        JPanel panel = new JPanel();
        public MyFrame() {
            super();
            setTitle("Key Event Example");
            setSize(500, 400);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            
            JTextField tf = new JTextField(20);
            tf.addKeyListener(new MyKeyListener());
            
            tf.requestFocus(); // event를 받도록 한다
                               // 어느 텍스트 필드에서 event를 받도록 하는지 설정해준다
                               // 만약 세 개의 텍스트 필드가 설정되있을 때
                               // 마우스로 하나의 텍스트 필드를 눌렀으면 그 텍스트 필드가 포커스 되지만
                               // 강제로 다른 곳을 포커스하고 싶으면 써주어야 한다
            panel.add(tf);
            add(panel);
            setVisible(true);
        }
        
        private class MyKeyListener implements KeyListener {
            // KeyListener 인터페이스에는 세 가지 메소드가 있는데 반드시 세 개 모두 작성해야 한다
            public void keyPressed(KeyEvent e) {
                display(e, "KeyPressed");
            }
            
            public void keyReleased(KeyEvent e) {
                display(e, "KeyReleased"); // 내용이 필요없다면 안써줘도 되지만 메소드 자체는 써주어야 한다
            }
            
            public void keyTyped(KeyEvent e) { // Unicode인 경우
                display(e, "KeyTyped");
            }
        }
        
        private void display(KeyEvent e, String s) {
            char ch = e.getKeyChar(); // a인지 b인지 등등 character를 받아옴
            int keyCode = e.getKeyCode(); // keyTyped 경우는 0, code 번호를 받아옴
            // alt 키나 ctrl 키, shift 키를 같이 눌렀는지 안눌렀는지 
            String modifiers = e.isAltDown() + " " + e.isControlDown() + " " + e.isShiftDown();
            System.out.println(s + " " + ch + " " + keyCode + " " + modifiers);
        }	
    }
    
    public class KeyEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    }

---

    import javax.imageio.ImageIO;
    import javax.swing.*;
    import java.awt.*;
    import java.awt.event.*;
    import java.io.*;
    import java.awt.image.BufferedImage;
    
    class MyPanel extends JPanel {
        private BufferedImage img = null;
        private int x = 200, y = 200;
        
        public MyPanel() {
            try {
                img = ImageIO.read(new File("car.jpg"));
            }
            catch(IOException e) {
                System.out.println("No image");
                System.exit(1);
            }
            
            addKeyListener(new MyKeyListener()); // panel이 키 입력을 받도록 한다
            this.requestFocus(); // JPanel은 default로 key event를 받을 수 없기 때문
            setFocusable(true); // key event를 받도록 한다
        }
        
        protected void paintComponent(Graphics g) { 
            super.paintComponent(g);
            Graphics2D g2 = (Graphics2D)g;
            g2.drawImage(img, x, y, null);
        }
        
        private class MyKeyListener implements KeyListener {
            public void keyPressed(KeyEvent e) {
                int keycode = e.getKeyCode();
                switch(keycode) { 
                // 마우스 화살표 키에 따라 위치를 변경시키고 싶다
                case KeyEvent.VK_UP : y -= 10; break;
                case KeyEvent.VK_DOWN : y += 10; break;
                case KeyEvent.VK_LEFT : x -= 10; break;
                case KeyEvent.VK_RIGHT : x += 10; break;
                
                }
                repaint();
            }
            
            // KeyListener 인터페이스에 선언되어 있는 메소드는 사용하지 않아도 정의해야 한다
            
            public void keyReleased(KeyEvent e) {
            }
            
            public void keyTyped(KeyEvent e) {
            }
        }
    }
    
    public class CarGameTest {
    
        public static void main(String[] args) {
            MyPanel p = new MyPanel();
        }
    }

---

    import java.awt.*;
    import javax.swing.*;
    import java.awt.event.*;
    
    class MyPanel extends JPanel {
        boolean flag = false;
        private int light_number = 0;
        MyListener listener = new MyListener();
        
        public MyPanel() {
            setLayout(new FlowLayout(FlowLayout.LEADING));
            JButton b = new JButton("traffic light turn on");
            b.addActionListener(listener);
            add(b, BorderLayout.SOUTH);
        }
        
        protected void paintComponent(Graphics g) {
            super.paintComponent(g);
            setLocation(90, 30);
            g.setColor(Color.BLACK);
            g.drawOval(100, 100, 100, 100);
            g.drawOval(100, 200, 100, 100);
            g.drawOval(100, 300, 100, 100);
            
            if(light_number == 0) {
                g.setColor(Color.RED);
                g.fillOval(100, 100, 100, 100);
            }
            else if(light_number == 1) {
                g.setColor(Color.GREEN);
                g.fillOval(100, 200, 100, 100);
            }
            else {
                g.setColor(Color.YELLOW);
                g.fillOval(100, 300, 100, 100);
            }
        }
        
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                if(++light_number >= 3)
                    light_number = 0;
                repaint();
            }
        }
    }
    
    class MyFrame extends JFrame {
        public MyFrame() {
            add(new MyPanel());
            setLocation(700, 300);
            setSize(500, 500);
            setTitle("Traffic Light");
            setVisible(true);
        }
    }
    
    public class ActionEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }

---

    MouseEvent - 1. MouseListener 2. MouseMotionListener
    
    // MouseListener interface와 MouseMotionListener interface를
    // implements 하는 경우 MouseListener에 선언된 메소드 5개와
    // MouseMotionListener에 선언된 2개의 메소드를 모두 구현해야 한다
    
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame {
        JPanel panel;
        
        public MyFrame() {
            setTitle("MouseEvent");
            setSize(300, 200);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            
            panel = new JPanel();
            panel.addMouseListener(new MyMouseListener());
            panel.addMouseMotionListener(new MyMotionListener());
            add(panel);
            setVisible(true);
        }
        
        private class MyMouseListener implements MouseListener {
            public void mousePressed(MouseEvent e) {
                display("Mouse pressed ", e);
            }
            public void mouseReleased(MouseEvent e) {
                display("Mouse Released ", e);
            }
            public void mouseClicked(MouseEvent e) {
                display("Mouse clicked ( # of clicks : " + e.getClickCount() + ")", e);
            }
            public void mouseEntered(MouseEvent e) {
                display("Mouse entered ", e);
            }
            public void mouseExited(MouseEvent e) {
                display("Mouse exited ", e);
            }
        }
        // MouseMotion은 시스템 overhead가 많기 때문에 따로 작성
        private class MyMotionListener implements MouseMotionListener {
            public void mouseDragged(MouseEvent e) {
                display("Mouse dragged ", e);
            }
            public void mouseMoved(MouseEvent e) {
                display("Mouse moved ", e);
            }
        }
        
        public void display(String s, MouseEvent e) {
            System.out.println(s + " x = " + e.getX() + " y = " + e.getY());
            System.out.println(e.getButton()); // left : 1, wheel : 2, right : 3
                                               // 어떤 버튼이 눌렸는지 반환
        }
    }
    
    
    public class MouseEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
    
        }
    
    }

---

    // MouseAdapter클래스나 MouseMotionAdapter를 상속받는 경우 필요한 메소드만 정의하면 된다
    // MouseAdapter나 MouseMotionAdapter는 interface가 아니라
    // class이기 때문에 반드시 상속을 받아야 한다
    
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyFrame extends JFrame {
        JPanel panel;
        
        public MyFrame() {
            setTitle("MouseEvent");
            setSize(300, 200);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            
            panel = new JPanel();
            panel.addMouseListener(new MyMouseListener());
            panel.addMouseMotionListener(new MyMotionListener());
            add(panel);
            setVisible(true);
        }
        // 만약 mouseClicked() 메소드 이외의 메소드는 필요가 없을 때
        // 일일이 다 빈 몸체로 선언하는 것은 불필요
        // MouseAdapter클래스를 이용하면 필요한 것만 정의해서 사용 가능
        private class MyMouseListener extends MouseAdapter {
            public void mouseClicked(MouseEvent e) {
                display("Mouse clicked ( # of clicks : " + e.getClickCount() + ")", e);
            }
        }
        // MouseMotionAdapter도 마찬가지이다
        private class MyMotionListener extends MouseMotionAdapter {
            public void mouseMoved(MouseEvent e) {
                display("Mouse moved ", e);
            }
        }
        
        public void display(String s, MouseEvent e) {
            System.out.println(s + " x = " + e.getX() + " y = " + e.getY());
            System.out.println(e.getButton()); // left : 1, wheel : 2, right : 3
                                               // 어떤 버튼이 눌렸는지 반환
        }
    }
    
    
    public class MouseEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
    
        }
    
    }

---
    
    import javax.swing.*;
    import java.awt.event.*;
    
    class MyFrame extends JFrame {
        JPanel panel = new JPanel();
        public MyFrame() {
            super();
            setTitle("Key Event Example");
            setSize(500, 400);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            
            JTextField tf = new JTextField(20);
            tf.addKeyListener(new MyKeyListener());
            
            tf.requestFocus(); 
            panel.add(tf);
            add(panel);
            setVisible(true);
        }
        // 필요한 부분만 오버라이딩해서 사용 : keyAdapter
        private class MyKeyListener extends KeyAdapter {
            public void keyPressed(KeyEvent e) {
                display(e, "KeyPressed");
            }
        }
        
        private void display(KeyEvent e, String s) {
            char ch = e.getKeyChar(); 
            int keyCode = e.getKeyCode(); 
            String modifiers = e.isAltDown() + " " + e.isControlDown() + " " + e.isShiftDown();
            System.out.println(s + " " + ch + " " + keyCode + " " + modifiers);
        }	
    }
    
    public class KeyEventTest {
    
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    }

---

    /*
    import javax.imageio.ImageIO;
    import javax.swing.*;
    import java.awt.*;
    import java.awt.event.*;
    import java.io.*;
    import java.awt.image.BufferedImage;
    
    class MyPanel extends JPanel {
        private BufferedImage img = null;
        private int x = 200, y = 200;
        
        public MyPanel() {
            try {
                img = ImageIO.read(new File("car.jpg"));
            }
            catch(IOException e) {
                System.out.println("No image");
                System.exit(1);
            }
            this.addMouseListener(new MyMouseListener());
            addMouseMotionListener(new MyMouseMotionListener());
        }
        
        protected void paintComponent(Graphics g) { 
            super.paintComponent(g);
            g.drawImage(img, x, y, null);
        }
        // MouseAdapter, MouseMotionAdapter 클래스를 상속 받아 필요한 메소드만 정의한다
        private class MyMouseListener extends MouseAdapter {
            public void mosuePressed(MouseEvent e) {
                x = e.getX();
                y = e.getY();
                repaint();
            }
        }
        
        private class MyMouseMotionListener extends MouseMotionAdapter {
            public void mouseDragged(MouseEvent e) {
                x = e.getX();
                y = e.getX();
                repaint();
            }
        }
    }
    
    public class CarGameTest extends JFrame {
        public CarGameTest() {
            setTitle("Car Game");
            setSize(1500, 1000);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            add(new MyPanel());
            setVisible(true);
        }
        public static void main(String[] args) {
            CarGameTest test = new CarGameTest();
        }
    }
    */
    
    import javax.imageio.ImageIO;
    import javax.swing.*;
    import java.awt.*;
    import java.awt.event.*;
    import java.io.*;
    import java.awt.image.BufferedImage;
    
    class MyPanel extends JPanel {
        private BufferedImage img = null;
        private int x = 200, y = 200;
        
        public MyPanel() {
            try {
                img = ImageIO.read(new File("car.jpg"));
            }
            catch(IOException e) {
                System.out.println("No image");
                System.exit(1);
            }
            this.addMouseListener(new MyMouseListener());
            addMouseMotionListener(new MyMouseMotionListener());
            addMouseWheelListener(new MyMouseWheelListener());
        }
        
        protected void paintComponent(Graphics g) { 
            super.paintComponent(g);
            g.drawImage(img, x, y, null);
        }
        // MouseAdapter, MouseMotionAdapter 클래스를 상속 받아 필요한 메소드만 정의한다
        private class MyMouseListener extends MouseAdapter {
            public void mosuePressed(MouseEvent e) {
                x = e.getX();
                y = e.getY();
                repaint();
            }
        }
        
        private class MyMouseMotionListener extends MouseMotionAdapter {
            public void mouseDragged(MouseEvent e) {
                x = e.getX();
                y = e.getY();
                repaint();
            }
        }
        // MouseWheelListener에는 메소드가 하나만 정의되어 있어서 Adapter클래스를 상속할 필요 없다
        private class MyMouseWheelListener implements MouseWheelListener {
            public void mouseWheelMoved(MouseWheelEvent e) {
                if(e.getWheelRotation() == 1) // 아래 : 1, 위 : -1
                    y += 10;
                else
                    y -= 10;
                repaint();
            }
        }
    }
    
    public class CarGameTest extends JFrame {
        public CarGameTest() {
            setTitle("Car Game");
            setSize(1500, 1000);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            add(new MyPanel());
            setVisible(true);
        }
        public static void main(String[] args) {
            CarGameTest test = new CarGameTest();
        }
    }

---

    import java.awt.*;
    import javax.swing.*;
    import java.awt.event.*;
    
    class Rectangle {
        int x, y, w, h;
    }
    
    class MyPanel extends JPanel {
        Rectangle[] array;
        int index = 0;
        
        public MyPanel() {
            array = new Rectangle[100];
            addMouseListener(new MyListener());
        }
        
        private class MyListener extends MouseAdapter {
            public void mousePressed(MouseEvent e) {
                if(index >= 100)
                    return;
                
                array[index] = new Rectangle();
                array[index].x = e.getX();
                array[index].y = e.getY();
                array[index].w = 20;
                array[index].h = 20;
                index++;
                repaint();
            }
        }
        
        public void paintComponent(Graphics g) {
            super.paintComponent(g);
            for(Rectangle r : array)
                if(r != null)
                    g.drawRect(r.x, r.y, r.w, r.h);
        }
    }
    
    public class CounterTest extends JFrame {
        public CounterTest() {
            setTitle("Rectangle");
            setSize(500, 400);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            setLocationRelativeTo(null);
            
            add(new MyPanel());
            setVisible(true);
        }
    
        public static void main(String[] args) {
            CounterTest t = new CounterTest();
        }
    
    }

---
   
    Swing 컴포넌트
    -JComponent : 거의 모든 컴포넌트가 JComponent를 상속받음
    -Object -> component -> container -> JComponent -> JLabel, JPanel, 등등
    
    import java.awt.event.*;
    import javax.swing.*;
    import java.awt.*;
    
    class MyPanel extends JPanel {
        private JLabel label;
        private JButton button;
        private int count = 0; // 짝수 혹은 홀수
        private ImageIcon icon = new ImageIcon("icon.jpg");
        private ImageIcon dog = new ImageIcon("dog.jpg");
        
        public MyPanel() {
            label = new JLabel("Press button to see image");
            button = new JButton("Image Button");
            button.setIcon(icon); // 버튼 위에 이미지
            button.addActionListener(new MyListener());
            button.setBorder(BorderFactory.createTitledBorder("Image")); // 외곽선(버튼에 적용), javax.swing
            setBorder(BorderFactory.createEtchedBorder()); // 외관선 모양잉이 파인 것 같은(패널에 적용), javax.swing
            add(button);
                    add(label);
        }
        
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                if(count % 2 == 0) { 
                    label.setIcon(dog); // 레이블 위에 이미지
                    label.setText(null);
                }
                else {
                    label.setIcon(null); // 그림을 없애고
                    label.setText("Press button to see Image"); // 텍스트를 보여줌
                }
                count++;
            }
        }
    }
    
    public class ImageLabelTest extends JFrame {
        public ImageLabelTest() {
        setTitle("Image Label");
        setSize(500, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        add(new MyPanel());
        setVisible(true);
        }
        public static void main(String[] args) {
            ImageLabelTest i = new ImageLabelTest();
        }
    
    }

---

    checkBox
    
    // JCheckBox는 여러개 선택 가능하고 ItemEvent 필요
    // ItemListener interface itemStateChanged 메소드 작성
    
    import java.awt.event.*;
    import javax.swing.*;
    import java.awt.*;
    
    class MyPanel extends JPanel {
        JCheckBox[] check = new JCheckBox[3];
        String[] names = { "dog", "cat", "bear" };
        JLabel[] label = new JLabel[3];
        ImageIcon[] icon = new ImageIcon[3];
        
        public MyPanel() {
            setLayout(new GridLayout(1, 4)); // 기본 패널에 1행 4열 짜리 패널을 만든다
            for(int i = 0; i < 3; i++) {
                check[i] = new JCheckBox(names[i]);
                check[i].addItemListener(new MyItemListener());
                label[i] = new JLabel(names[i] + ".jpg");
                icon[i] = new ImageIcon(names[i] + ".jpg");
            }
            
            JPanel namePanel = new JPanel(new GridLayout(3, 1)); // 왼쪽 레이블에 체크박스를 세 개 만들기 위해서
            for(int i = 0; i < 3; i++) {
                namePanel.add(check[i]);
            }
            add(namePanel);
            add(label[0]);
            add(label[1]);
            add(label[2]);
        }
        // 내부 클래스로 만드는 것이 이벤트를 처리하는데 훨씬 더 간편하다
        private class MyItemListener implements ItemListener {
            public void itemStateChanged(ItemEvent e) {
                for(int i = 0; i < check.length; i++) {
                    if(e.getSource() == check[i]) {
                        if(e.getStateChange() == ItemEvent.DESELECTED) { // 선택 -> 선택 해제
                            label[i].setIcon(null); // 선택이 해제되면 그림을 없애고
                            label[i].setText(names[i] + ".jpg"); // 텍스트가 나오게 한다
                        }
                        else {
                            label[i].setIcon(icon[i]); // 선택이되면 그림을 나타내고
                            label[i].setText(null); // 텍스트를 없앤다
                        }
                    }
                }
            }
        }
    }
    
    public class CheckBoxTest extends JFrame {
        public CheckBoxTest() {
            setSize(1000, 800);
            setTitle("Check BOx");
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            add(new MyPanel());
    //		pack();
            setVisible(true);
        }
        
        public static void main(String[] args) {
            new CheckBoxTest();
        }
    
    }

---

    // JRadioButton은 체크박스와 비슷하지만 한개만 선택 가능하고 ActionEvent 발생
    // 반드시 ButtonGroup에 포함시킨다
    // BorderFactory 클래스의 정적 메소드
    // 1. createLineBorder(Color), createLineBorder(Color, int) : 직선으로 된 경계 생성
       2. createTitleBorder(String) : 제목이 붙여진 경계 생성, 문자열 매개변수가 제목
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel {
        private JRadioButton small, medium, large;
        private JLabel text;
        private JPanel top, size, result;
        private MyListener listener = new MyListener();
        
        public MyPanel() {
            setLayout(new BorderLayout());
            top = new JPanel();
            JLabel label = new JLabel("Select coffee size");
            top.add(label);
            add(top, BorderLayout.NORTH);
            top.setBorder(BorderFactory.createEtchedBorder());
            
            size = new JPanel();
            small = new JRadioButton("Small");
            medium = new JRadioButton("Medium");
            large = new JRadioButton("Large");
            ButtonGroup group = new ButtonGroup(); // JRadioButton을 group로 지정해야 한다
            group.add(small); group.add(medium); group.add(large);
            small.addActionListener(listener);
            medium.addActionListener(listener);
            large.addActionListener(listener);
            size.add(small); size.add(medium); size.add(large);
            add(size, BorderLayout.CENTER);
            size.setBorder(BorderFactory.createTitledBorder("Size"));
            
            result = new JPanel();
            text = new JLabel("Size is not selected");
            text.setForeground(Color.red); // 문자열의 색깔
            result.setBackground(Color.green); // 배경 색깔
            result.add(text);
            add(result, BorderLayout.SOUTH);
            result.setBorder(BorderFactory.createRaisedSoftBevelBorder());
        }
        
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                if(e.getSource() == small) 
                    text.setText("Small");
                else if(e.getSource() == medium) 
                    text.setText("Medium");
                else
                    text.setText("Large");
                System.out.println(small.isSelected() + " " + medium.isSelected() + " " + large.isSelected());
            }
        }
    }
    
    public class RadioButtonTest extends JFrame {
        public RadioButtonTest() {
            setSize(800, 600);
            setTitle("RadioButton");
            setLocationRelativeTo(null);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            add(new MyPanel());
            pack();
            setVisible(true);
        }
    
        public static void main(String[] args) {
            RadioButtonTest rbt = new RadioButtonTest();
        }
    
    }

---

    스크롤 패널
    
    // JTextField는 ActionEvent 발생
    
    import java.awt.*;
    import java.awt.event.*;
    import javax.swing.*;
    
    class MyPanel extends JPanel {
        private JTextField field; // 텍스트 필드(간단한 줄)
        private JTextArea area; // 단순 텍스트 영역(여러 줄)
        
        public MyPanel() {
            field = new JTextField(20);
            field.addActionListener(new MyListener());
            area = new JTextArea(10, 20);
            
            JScrollPane pane = new JScrollPane(area, // 수평, 수직 스크롤바
                    JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED, // 필요할 때만 스크롤바가 나타남 
                    JScrollPane.HORIZONTAL_SCROLLBAR_ALWAYS); // 항상 스크로로바가 나타남
            add(field);
            add(pane); // area가 포함된 JScrollPane 객체를 add해야지
                       // area를 add하면 스크롤바가 나타나지 않음
        }
        
        private class MyListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                String str = field.getText();
                area.append(str + "\n");
                field.selectAll(); // 다음 입력을 위한 현재 문자열 삭제 준비(안써주면 일일이 지워야함)
            }
        }
    }
    
    class MyFrame extends JFrame {
        public MyFrame() {
            setTitle("Text Test");
            setSize(700, 500);
            setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            JScrollPane p = new JScrollPane(new MyPanel(), // 수평, 수직 스크롤바
                    JScrollPane.VERTICAL_SCROLLBAR_ALWAYS,
                    JScrollPane.HORIZONTAL_SCROLLBAR_ALWAYS);
            add(p);
    //      add(MyPanel()); // JPanel을 add하지 않고 JScrollPane을 add한다 
            setVisible(true);
        }
    }
    
    public class TextTest {
        public static void main(String[] args) {
            MyFrame f = new MyFrame();
        }
    
    }
