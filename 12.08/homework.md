## Chatting Program

- 미완성

### Result

![chatting](https://user-images.githubusercontent.com/43839669/101497500-66663880-39ae-11eb-9ce5-df062873bc93.JPG)

### Common.java

```java

package day32.homework;

import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.Socket;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public abstract class Common extends JFrame implements Runnable {
	private String name;
	private BufferedWriter bw;
	private BufferedReader br;
	private Socket socket;

	private JFrame frame;
	private JButton btn;
	private JTextArea textArea;
	private JTextField textField;
	boolean bool = true;
	// private String input;

	public void setSocket(Socket socket) {
		this.socket = socket;
	}

	public Socket getSocket() {
		return socket;
	}

	public Common(String name) {
		super(name + " CHATTING");
		this.name = name;

		setSize(300, 500);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setLocation(500, 500);

		textArea = new JTextArea();

		JPanel south = new JPanel();
		south.setLayout(new BorderLayout());
		textField = new JTextField();
		btn = new JButton("전송");
		south.add(textField, BorderLayout.CENTER);
		south.add(btn, BorderLayout.EAST);

		this.add(textArea, BorderLayout.CENTER);
		this.add(south, BorderLayout.SOUTH);

		setVisible(true);
		textField.requestFocus();

		execute();
	}

	@Override
	public void run() {
		String input = "초기";
		try {
			// while((input=br.readLine()) != null) {
			System.out.println(name + " : " + input);

		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	public void actionPerformed(ActionEvent e) {
		btn = (JButton) e.getSource();
		textArea.setText(this.name + ":" + textField.getText());
		textField.setText(null);
	}

	private void execute() {

		try {
			init();
			br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

			String input;
			while (bool) {

				btn.addActionListener(new ActionListener() {

					@Override
					public void actionPerformed(ActionEvent e) {
						String string = textField.getText();

						try {
							bw.write(string + "\n");
							bw.flush();
							bool = false;
						} catch (IOException e1) {
							e1.printStackTrace();
						}

						String chat = textArea.getText();
						textField.setText(null);
						textArea.setText(chat);

					}
				});

			}

			new Thread(this).start();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				if (bw != null)
					bw.close();
				if (br != null)
					br.close();
				if (socket != null)
					socket.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}

	}

	protected abstract void init() throws IOException;

}
```


### Server.java

```java
package day32.homework;

import java.io.IOException;
import java.net.ServerSocket;

public class Server extends Common implements Runnable {

	private ServerSocket sSocket;

	@Override
	protected void init() throws IOException {
		sSocket = new ServerSocket(50000);
		System.out.println("서버 소켓 생성!");
		setSocket(sSocket.accept());
		System.out.println("클라이언트와 연결되었다. 클라이언트 IP : " + getSocket().getInetAddress());		
	}

	public Server() {
		super("Server");
	}

	public static void main(String[] args) {
		new Server();
	}
}
```


### Client.java

```java
package day32.homework;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.UnknownHostException;

import javax.swing.JOptionPane;

public class Client extends Common implements Runnable{
	
	
	protected void init() throws UnknownHostException, IOException {
		setSocket(new Socket("127.0.0.1", 50000));
		System.out.println("서버와 연결되었다!");
	}
	
	public Client() {
		super("Client");
	}
	
	
	public static void main(String[] args) {
		new Client();
	}
}

```
