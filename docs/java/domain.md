# java将域名解析为ip

```
import java.net.InetAddress;
import java.net.UnknownHostException;
 
public class DomainToIP {
    public static void main(String[] args) {
        String domain = "www.example.com";
        try {
            InetAddress inetAddress = InetAddress.getByName(domain);
            String ip = inetAddress.getHostAddress();
            System.out.println("IP Address: " + ip);
        } catch (UnknownHostException e) {
            System.err.println("Cannot resolve hostname: " + domain);
        }
    }
}
```