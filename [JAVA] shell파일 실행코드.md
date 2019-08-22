# command를 통한 shell실행

```java
package sh_test;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;

public class Main {
	public static void main(String[] args) {
		System.out.println("main Test");
		System.out.println(args.length);
		try {
			shellCmd("sh /opt/test/sign/sign.sh");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	public static void shellCmd(String command) throws Exception {
		Runtime runtime = Runtime.getRuntime();
		Process process = runtime.exec(command);
		InputStream is = process.getInputStream();
		InputStreamReader isr = new InputStreamReader(is);
		BufferedReader br = new BufferedReader(isr);
		String line;
		while ((line = br.readLine()) != null) {
			System.out.println(line);
		}
	}
}

```

