## 1.1 Latar Belakang

Game Merakit PC

Game sederhana yang masih terhubung dengan dunia informatika yaitu merakit pc

Konsep game ini yaitu awal mulanya terlintas dibenak saya ketika saya mengikuti mata kuliah praktikum dasar pemrograman yang mana pada pertemuan ke 6 belajar tentang game sederhana yang hanya tampil di CLI yaitu game jualan kue, game jualan kue ini hanya dapat maju mundur dan atas bawah saja. Saya terpikirkan untuk merubah konsepnya menjadi game yang terhubung pada dunia informatika yaitu merakit pc. Lebih tepatnya gambaran game ini yaitu seorang mahasiswa yang menginginkan pc tetapi harus mengumpulkan beberapa hardware dilevel yang berbeda dan tantangan yang berbeda. Dan jika semua hardware sudah terkumpul, pengguna merasa gembira dan senang dapat merakit dan menjalankan pcnya
Game ini mempunyai kesimpulan dan pesan tersembunyi yaitu ketika seorang mahasiswa menginginkan sebuah pc , mereka harus susah payah menabung dan melewati beberapa rintangan untuk mengumpulkan part part pc / hardware yang mereka ingin agar terciptanya sebuah PC yang mereka inginkan

## 1.2. Deksripsi Teknologi Informasi
Game yang saya buat ini masih berhubungan dengan teknologi informasi karena game ini mengenali sebuah perangkat keras komputer, yang mana komputer ini adalah salah satu teknologi informasi yang digunakan hingga saat ini

## 1.3. Branding
Game ini cocok untuk para anak sekolah / siswa SMP-SMK karena siswa sekarang senang bermain game dan mengharapkan memiliki PC dirumahnya, di game ini diajarkan untuk mengenali beberapa hardware yang terdapat pada komputer beserta fungsi fungsi nya, adanya game ini saya berharap siswa diumuran SMP sudah dapat mengenali hardware yang berada pada komputer dan dapat merakitnya sendiri

Nama game : RakitPc

Tagline: Menghadang dan menghadapi rintangan untuk merakit sebuah PC

Campaign: 

Target user:

Usia 7+
- Seorang yang senang terhadap mesin teknologi informasi

- Seorang yang senang melewati rintangan di setiap levelnya

- Seorang yang ingin belajar tentang perangkat keras pada komputer

- Seorang yang ingin mencoba merakit pc pertamanya dirumah

User experience theme:

- Mudah

- Sederhana

- Menyenangkan

- Mendapat wawasan

Warna: perpaduan hijau dan biru terlihat sedikit elegant

## 2. User Story
Sebagai | Saya ingin bisa| Sehingga |Prioritas
---|---|---|---
User | Menjalankan pengguna maju mundur | User dapat melaju hingga finish| ⭐⭐⭐⭐⭐
User | Melawan musuh / menembak | User dapat maju ke level selanjutnya| ⭐⭐⭐⭐⭐
User | Mengklaim hardware / perangkat keras komputer|User dapat mengumpulkan hardware| ⭐⭐⭐⭐⭐


## 3. Struktur Data

    User ||--o{ MENEMBAK : tembak
    User ||--|{ MELAJU : maju
    User ||--o{ MENGCLAIM : claim
    User ||--o{ FINISH : finish

## 4. Arsitektur Sistem

-

## 5. Teknologi, Library, dan Framework
Library Menggunakan java swing

## 6. Desain User Experience dan User Interface

https://www.figma.com/file/FhkU6ycUMyRrB7y68KSBMY/Untitled?type=design&node-id=0%3A1&mode=design&t=tfPyw67yNBDT3V2R-1

## 7. Demonstrasi Video
import java.awt.*;
import java.awt.event.*;
import java.awt.image.*;
import javax.swing.*;
import java.io.File;
import java.io.IOException;
import javax.imageio.ImageIO;
import java.util.Random;

public class GameRpc extends JPanel {

    private Player player = new Player();
    private Musuh musuh = new Musuh();

    private BufferedImage asetImageKarakter, asetImageMusuh;

    private int nyawaKarakter = 3;
    private int nyawaMusuh = 3;

    public void mulaiHitungan() {
        int detikBerjalan = 0;

        while (detikBerjalan >= 0) {
            System.out.println(detikBerjalan);
            detikBerjalan++;

            this.repaint();

            if (player.tabrakMusuh(musuh)) {
                if (player.energi > 0) {
                    player.energi--;
                } else {
                    System.out.println("Game Over!");
                    System.exit(0);
                }
                System.out.println("Nyawa Karakter: " + player.energi);

                player.pantulkan();
                musuh.gerakRandom();
            } else {
                musuh.gerakMenuju(player); 
            }

            musuh.periksaTembok(this);

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public GameRpc() {
        setBackground(new Color(173, 216, 230));

        try {
            asetImageKarakter = ImageIO.read(new File("./karakter.png"));
            asetImageMusuh = ImageIO.read(new File("./musuh.png"));
        } catch (IOException e) {
            System.out.println("Gagal membuka file image");
        }

        GameRpc gameIni = this;

        JButton closeButton = new JButton("Close");
        closeButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                System.exit(0); 
            }
        });

        setLayout(new BorderLayout());
        add(closeButton, BorderLayout.NORTH);

        KeyboardFocusManager.getCurrentKeyboardFocusManager()
                .addKeyEventDispatcher(new KeyEventDispatcher() {
                    @Override
                    public boolean dispatchKeyEvent(KeyEvent e) {
                        switch (e.getKeyCode()) {
                            case KeyEvent.VK_UP:
                                gameIni.player.majuY(-1);
                                break;
                            case KeyEvent.VK_DOWN:
                                gameIni.player.majuY(1);
                                break;
                            case KeyEvent.VK_LEFT:
                                gameIni.player.majuX(-1);
                                break;
                            case KeyEvent.VK_RIGHT:
                                gameIni.player.majuX(1);
                                break;
                        }
                        gameIni.repaint();
                        return false;
                    }
                });
    }

    public static void main(String[] args) {
        JFrame f = new JFrame();

        GameRpc game = new GameRpc();
        game.setPreferredSize(new Dimension(800, 600));
        f.add(game);

        f.setTitle("Game RPC");
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        f.setExtendedState(JFrame.MAXIMIZED_BOTH); 
        f.setUndecorated(true); 
        f.setVisible(true);

        game.mulaiHitungan();
    }

    public void paint(Graphics g) {
        super.paint(g);
        
        g.drawImage(this.asetImageMusuh, this.musuh.posisi[0], this.musuh.posisi[1], this);

        g.drawImage(this.asetImageKarakter, this.player.posisi[0], this.player.posisi[1], this);

        g.setColor(Color.BLACK);
        g.fillRect(10, 10, getWidth() - 20, 10);
        g.fillRect(10, 10, 10, getHeight() - 20); 
        g.fillRect(getWidth() - 20, 10, 10, getHeight() - 20);
        g.fillRect(10, getHeight() - 20, getWidth() - 20, 10); 
    }
}

class Player {
    int[] posisi = new int[] { 800 - lebarKarakter, 600 - tinggiKarakter }; 
    int pengaliLangkahMaju = 16;
    static final int lebarKarakter = 50;
    static final int tinggiKarakter = 50; 
    int energi = 100; // Tambahkan variabel energi dengan nilai awal 100

    private int[] posisiSebelumPemantulan = new int[2];
    private boolean sedangPemantulan = false;

    void majuX(int xRelatif) {
        int newX = posisi[0] + (xRelatif * pengaliLangkahMaju);
        if (newX >= 10 && newX <= 800 - 10 - lebarKarakter) {
            if (!sedangPemantulan) {
                posisiSebelumPemantulan[0] = posisi[0];
                posisiSebelumPemantulan[1] = posisi[1];
            }

            posisi[0] = newX;
        }
    }

    void majuY(int yRelatif) {
        int newY = posisi[1] + (yRelatif * pengaliLangkahMaju);
        if (newY >= 10 && newY <= 600 - 10 - tinggiKarakter) {
            if (!sedangPemantulan) {
                posisiSebelumPemantulan[0] = posisi[0];
                posisiSebelumPemantulan[1] = posisi[1];
            }

            posisi[1] = newY;
        }
    }

    boolean tabrakMusuh(Musuh musuh) {
        Rectangle karakterBounds = new Rectangle(posisi[0], posisi[1], lebarKarakter, tinggiKarakter);
        Rectangle musuhBounds = new Rectangle(musuh.posisi[0], musuh.posisi[1], Musuh.lebarMusuh, Musuh.tinggiMusuh);
        return karakterBounds.intersects(musuhBounds);
    }

    void pantulkan() {
        posisi[0] = posisiSebelumPemantulan[0];
        posisi[1] = posisiSebelumPemantulan[1];

        pengaliLangkahMaju *= -1;

        sedangPemantulan = true;

        Timer timer = new Timer(500, new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                sedangPemantulan = false;
            }
        });
        timer.setRepeats(false);
        timer.start();
    }
}

class Musuh {
    int[] posisi = new int[] { 100, 100 };
    int pengaliLangkahMaju = 8;
    static final int lebarMusuh = 50;
    static final int tinggiMusuh = 50;

    void majuX(int xRelatif) {
        posisi[0] += (xRelatif * pengaliLangkahMaju);
    }

    void majuY(int yRelatif) {
        posisi[1] += (yRelatif * pengaliLangkahMaju);
    }

    void periksaTembok(GameRpc game) {
        if (posisi[0] <= 10 || posisi[0] >= game.getWidth() - 20 - lebarMusuh) {
            pengaliLangkahMaju *= -1; 
        }
        if (posisi[1] <= 10 || posisi[1] >= game.getHeight() - 20 - tinggiMusuh) {
            pengaliLangkahMaju *= -1;
        }
    }

    void gerakMenuju(Player karakter) {
        int deltaX = karakter.posisi[0] - posisi[0];
        int deltaY = karakter.posisi[1] - posisi[1];

        int arahX = Integer.compare(deltaX, 0);
        int arahY = Integer.compare(deltaY, 0);

        majuX(arahX);
        majuY(arahY);
    }

    void gerakRandom() {
        Random random = new Random();

        int arahX = random.nextInt(2) * 2 - 1;

        int arahY = random.nextInt(2) * 2 - 1;
        majuX(arahX);
        majuY(arahY);
    }
}

![karakter](https://github.com/azkaasst/azkaasst/assets/146172353/b0dd656b-01eb-455a-b9f2-1e724375a19b)
![musuh](https://github.com/azkaasst/azkaasst/assets/146172353/dadd3258-9eeb-4574-a562-dfbb62943e46)




## 8. Bagaimana mesin komputasi dan sistem operasi berperan dalam produk teknologi informasimu ?

Link youtube nya di detik jawaban ini.

## 9. Bagaimana algoritma, struktur data, dan bahasa pemrograman berperan dalam produk teknologi informasimu ?

Link youtube nya di detik jawaban ini

## 10. Bagaimana metode pengembangan perangkat lunak / Software Development Life Cycle berperan dalam produk teknologi informasimu ?

Link youtube nya di detik jawaban ini

## 11. Bagaimana database / sistem basis data berperan dalam produk teknologi informasimu ?

Link youtube nya di detik jawaban ini
