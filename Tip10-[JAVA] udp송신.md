# UDP 송신

간단하게 udp 통신 테스트로 보낼 수 있는 코드입니다. 아이피, 포트번호 변경하고 msg만 기입하시면 됩니다~

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;
import java.net.UnknownHostException;
 
public class Testudp {
 
    public static void main(String[] args) {
            try {
                InetAddress ip = InetAddress.getByName("localhost");
                //UDP 순서
                //소켓생성
                DatagramSocket socket = new DatagramSocket();
                //보낼 데이터를 바이트로 배열로 중비.
                 String msg = "[[\"CLIENT_INFO\":[\"com_name\":\"YOONHYEJOUNG\":\"account_name\":\"Administrator\":\"mac\":\"E0-D5-5E-DE-D3-4A\":\"ip\":\"192.168.153.1\":\"os_name\":\"Windows 10 Enterprise\":\"os_bit\":\"64bit\":\"os_ver\":\"1803\":\"sp_ver\":\"0\":\"agent_ver\":\"[v1.161025.2]\"]:\"PROCESS_INFO\":[\"ppproc_name\":\"conhost.exe\":\"pproc_path\":\"(null)\":\"pproc_size\":\"12345\":\"pproc_hash\":\"dfsjmor123dsjfs9u0sam2\":\"proc_name\":\"conhost.exe\":\"proc_path\":\"(null)\":\"proc_size\":434704:\"proc_hash\":\"dfsjmor123dsjfs9u0sam2\":\"proc_type\":\"(null)\":\"proc_acction\":2:\"tm\":\"(null)\"]]]";

                 byte[] buf = msg.getBytes("UTF-8");
                
                 //준비한 바이트 배열을 목적지주소와 목적지 포트번호를 통해 패킷으로 준비.
                 int port = 9999;
                 DatagramPacket p = new DatagramPacket(buf, buf.length,ip, port);
                
                 //소켓을 통해 패킷전송.
                 socket.send(p);
                
            } catch (UnknownHostException e) { //주소값이 없을때 발생하는 예외
                e.printStackTrace();
            } catch (SocketException e) { //소켓  에러 예외
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }       
    }
}

```

