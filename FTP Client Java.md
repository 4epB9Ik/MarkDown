**ChatGPT**: java ftp клиент
посоветуй простейшую библиотеку с возможностью заливать zip файл в папку и зачищать эту папку при необходимости от zip архивов

--------
**ChatGPT**: java ftp клиент
посоветуй простейшую библиотеку с возможностью заливать zip файл в папку и зачищать эту папку при необходимости от zip архивов

--------
**ChatGPT**: Для создания простого Java FTP клиента с возможностью заливки zip файлов в папку на FTP сервере и удаления zip архивов из этой папки, вы можете использовать библиотеку Apache Commons Net. Эта библиотека предоставляет простой интерфейс для работы с FTP протоколом.

Ниже приведен пример кода, который демонстрирует загрузку zip файла на FTP сервер и удаление zip архивов из указанной директории:java
```java
import org.apache.commons.net.ftp.FTP;
import org.apache.commons.net.ftp.FTPClient;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FTPClientExample {

    public static void main(String[] args) {
        String server = "ftp.example.com";
        int port = 21;
        String user = "username";
        String pass = "password";
        String remoteDirPath = "/path/to/remote/directory";
        String localFilePath = "/path/to/local/file.zip";

        FTPClient ftpClient = new FTPClient();
        try {
            ftpClient.connect(server, port);
            ftpClient.login(user, pass);
            ftpClient.enterLocalPassiveMode();
            ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

            // Загрузка zip файла на FTP сервер
            File localFile = new File(localFilePath);
            String remoteFilePath = remoteDirPath + "/" + localFile.getName();
            FileInputStream inputStream = new FileInputStream(localFile);
            boolean uploaded = ftpClient.storeFile(remoteFilePath, inputStream);
            inputStream.close();
            if (uploaded) {
                System.out.println("Zip файл успешно загружен на FTP сервер.");
            } else {
                System.out.println("Ошибка при загрузке zip файла на FTP сервер.");
            }

            // Удаление zip архивов из указанной директории на FTP сервере
            FTPFile[] files = ftpClient.listFiles(remoteDirPath);
            for (FTPFile file : files) {
                if (file.getName().endsWith(".zip")) {
                    String deleteFilePath = remoteDirPath + "/" + file.getName();
                    boolean deleted = ftpClient.deleteFile(deleteFilePath);
                    if (deleted) {
                        System.out.println("Zip файл " + file.getName() + " успешно удален с FTP сервера.");
                    } else {
                        System.out.println("Ошибка при удалении zip файла " + file.getName() + " с FTP сервера.");
                    }
                }
            }

            ftpClient.logout();
        } catch (IOException ex) {
            System.out.println("Ошибка: " + ex.getMessage());
            ex.printStackTrace();
        } finally {
            try {
                if (ftpClient.isConnected()) {
                    ftpClient.disconnect();
                }
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
    }
}
```

Обратите внимание, что вам нужно заменить значения переменных ```server```, ```port```, ```user```, ```pass```, ```remoteDirPath``` и ```localFilePath``` на соответствующие значения для вашего FTP сервера и локального файла.

--------
**ChatGPT**: Для создания простого Java FTP клиента с возможностью заливки zip файлов в папку на FTP сервере и удаления zip архивов из этой папки, вы можете использовать библиотеку Apache Commons Net. Эта библиотека предоставляет простой интерфейс для работы с FTP протоколом.

Ниже приведен пример кода, который демонстрирует загрузку zip файла на FTP сервер и удаление zip архивов из указанной директории:java
```java
import org.apache.commons.net.ftp.FTP;
import org.apache.commons.net.ftp.FTPClient;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FTPClientExample {

    public static void main(String[] args) {
        String server = "ftp.example.com";
        int port = 21;
        String user = "username";
        String pass = "password";
        String remoteDirPath = "/path/to/remote/directory";
        String localFilePath = "/path/to/local/file.zip";

        FTPClient ftpClient = new FTPClient();
        try {
            ftpClient.connect(server, port);
            ftpClient.login(user, pass);
            ftpClient.enterLocalPassiveMode();
            ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

            // Загрузка zip файла на FTP сервер
            File localFile = new File(localFilePath);
            String remoteFilePath = remoteDirPath + "/" + localFile.getName();
            FileInputStream inputStream = new FileInputStream(localFile);
            boolean uploaded = ftpClient.storeFile(remoteFilePath, inputStream);
            inputStream.close();
            if (uploaded) {
                System.out.println("Zip файл успешно загружен на FTP сервер.");
            } else {
                System.out.println("Ошибка при загрузке zip файла на FTP сервер.");
            }

            // Удаление zip архивов из указанной директории на FTP сервере
            FTPFile[] files = ftpClient.listFiles(remoteDirPath);
            for (FTPFile file : files) {
                if (file.getName().endsWith(".zip")) {
                    String deleteFilePath = remoteDirPath + "/" + file.getName();
                    boolean deleted = ftpClient.deleteFile(deleteFilePath);
                    if (deleted) {
                        System.out.println("Zip файл " + file.getName() + " успешно удален с FTP сервера.");
                    } else {
                        System.out.println("Ошибка при удалении zip файла " + file.getName() + " с FTP сервера.");
                    }
                }
            }

            ftpClient.logout();
        } catch (IOException ex) {
            System.out.println("Ошибка: " + ex.getMessage());
            ex.printStackTrace();
        } finally {
            try {
                if (ftpClient.isConnected()) {
                    ftpClient.disconnect();
                }
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
    }
}
```

Обратите внимание, что вам нужно заменить значения переменных ```server```, ```port```, ```user```, ```pass```, ```remoteDirPath``` и ```localFilePath``` на соответствующие значения для вашего FTP сервера и локального файла.

