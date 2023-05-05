import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.Scanner;

public class BankIKLC {
    static ArrayList<Rekening> rekening = new ArrayList<>();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean isRunning = true;

        while (isRunning) {
            System.out.println(" ----------------------------");
            System.out.println(" Selamat Datang di Bank IKLC!");
            System.out.println(" ----------------------------");
            
            System.out.println(" Choose an option: ");
            System.out.println("1. Mendaftar Akun");
            System.out.println("2. Mengirim Uang");
            System.out.println("3. Menyetor Uang");
            System.out.println("4. Exit");

            int option = scanner.nextInt();
            scanner.nextLine(); 

            switch (option) {
                
                case 1:
                    System.out.println("------------------");
                    System.out.println("Pendaftaran Akun :");
                    System.out.println("------------------");
                    
                    System.out.print("Nama: ");
                    String name = scanner.nextLine();
                    
                    System.out.print("Saldo Awal : ");
                    double saldoawal = scanner.nextDouble();
                    scanner.nextLine();
                    
                    Rekening newAccount = new Rekening (name, saldoawal);
                    rekening.add(newAccount);
                    System.out.println("----------------------------");
                    System.out.println(" Pendaftaran Akun Berhasil! ");
                    System.out.println(newAccount.toString());
                    break;
                    
                    
                case 2:
                    System.out.println("-----------------");
                    System.out.println("Pengiriman Uang :");
                    System.out.println("-----------------");
                    
                    System.out.print("No.Rekening Akun Pengirim : ");
                    int Noakunpengirim = scanner.nextInt();
                    scanner.nextLine(); 
                    
                    System.out.print("No.Rekening Akun Penerima : ");
                    int Noakunpenerima = scanner.nextInt();
                    scanner.nextLine(); 
                
                    double jumlahdikirim;
                    while (true) {
                        try {
                            System.out.print("Jumlah Uang yang Ingin Dikirim : ");
                            jumlahdikirim = Double.parseDouble(scanner.nextLine());
                            if (jumlahdikirim <= 0) {
                                throw new NumberFormatException();
                            }
                            break;
                        } catch (NumberFormatException e) {
                            System.out.println("Angka tidak valid. Silakan coba lagi.");
                        }
                    }
                    
                    Rekening akunpengirim = getRekening(Noakunpengirim);
                    Rekening akunpenerima = getRekening(Noakunpenerima);
                    if (akunpengirim == null) {
                        System.out.println("Akun Pengirim Tidak Dapat Ditemukan");
                    } 
                    else if (akunpenerima == null) {
                        System.out.println("Akun Penerima Tidak Dapat Ditemukan");
                    } 
                    else if (akunpengirim.getBalance() < jumlahdikirim) {
                        System.out.println("Jumlah Uang Tidak Mencukupi ");
                    } 
                    else {
                        akunpengirim.kurang(jumlahdikirim);
                        akunpenerima.nambah(jumlahdikirim);
                        System.out.println("---------------------");
                        System.out.println(" Transaksi Berhasil! ");
                        System.out.println(akunpengirim.toString());
                        System.out.println(akunpenerima.toString());
                    }
                    break;
                    
                    
                case 3:
                    System.out.println("------------------");
                    System.out.println("Penyetoran Uang  :");
                    System.out.println("------------------");

                    System.out.print("No. Akun: ");
                    int noakun = scanner.nextInt();
                    scanner.nextLine();

                    double amountSetor;
                    while (true) {
                        try {
                            System.out.print("Jumlah Uang yang Ingin Disetor : ");
                            amountSetor = Double.parseDouble(scanner.nextLine());
                            if (amountSetor <= 0) {
                                throw new NumberFormatException();
                            }
                            break;
                        } catch (NumberFormatException e) {
                            System.out.println("Angka tidak valid, Silakan coba lagi!");
                        }
                    }
                    
                    Rekening account = getRekening(noakun);
                    if (account == null) {
                        System.out.println("Akun Tidak Dapat Ditemukan.");
                    } else {
                        account.nambah(amountSetor);
                        System.out.println("----------------------");
                        System.out.println(" Penyetoran Berhasil! ");
                        System.out.println(account.toString());
                    }
                    break;
                    
                    
                case 4:
                    System.out.println(" ThankYou !! ");
                    isRunning = false;
                    break;
                    
                default:
                    System.out.println(" Opsi Tidak Valid !, Silahkan Pilih Ulang ");
                    break;
            }
        }
    }

    public static Rekening getRekening(int noakun) {
        for (Rekening bankAccount : rekening) {
            if (bankAccount.getNoAkun() == noakun) {
                return bankAccount;
            }
        }
        return null;
    }
}

    class Rekening {
        private String nama;
        private int noakun;
        private double balance;
        private LocalDateTime registrationDate;

    public Rekening(String nama, double saldoawal) {
        this.nama = nama;
        this.balance = saldoawal;
        this.registrationDate = LocalDateTime.now();
        this.noakun = generateNoAkun();
    }

    public String getNama() {
        return nama;
    }

    public int getNoAkun() {
        return noakun;
    }

    public double getBalance() {
        return balance;
    }

    public LocalDateTime getRegistrationDate() {
        return registrationDate;
    }

    public void nambah(double amount) {
        balance += amount;
    }

    public void kurang(double amount) {
        balance -= amount;
    }


    public String toString() {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String formattedDate = registrationDate.format(formatter);
    return  "Nama      : " + nama + "\n" +
            "No. Akun  : " + noakun + "\n" +
            "Saldo     : " + balance + "\n" +
            "Tanggal Registrasi    : " + formattedDate + "\n";
    }

    private int generateNoAkun() {
        boolean isUnique;
        int noakun;
        do {
            noakun = (int) (Math.random() * 90000) + 100000;
            isUnique = true;
        for (Rekening bankAccount : BankIKLC.rekening) {
            if (bankAccount.getNoAkun() == noakun) {
                isUnique = false;
                break;
            }
        }
    }       while (!isUnique);
            return noakun;
    }
}
