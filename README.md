# cbitirme-devi1
#include <stdio.h>
#include <stdlib.h>

#define MAX_USERS 100
#define MAX_NAME_LENGTH 50

typedef struct {
    int accountNumber;
    char name[MAX_NAME_LENGTH];
    double balance;
} User;

User users[MAX_USERS];
int numUsers = 0;

void registerUser() {
    if (numUsers >= MAX_USERS) {
        printf("Hesap oluşturma limitine ulaşıldı.\n");
        return;
    }

    User newUser;

    printf("Adınızı girin: ");
    scanf("%s", newUser.name);

    printf("Hesap numaranızı girin: ");
    scanf("%d", &newUser.accountNumber);

    printf("Başlangıç bakiyenizi girin: ");
    scanf("%lf", &newUser.balance);

    users[numUsers] = newUser;
    numUsers++;

    printf("Hesap başarıyla oluşturuldu.\n");
}

void updateUser() {
    int accountNumber;
    printf("Güncellenecek hesap numarasını girin: ");
    scanf("%d", &accountNumber);

    int i;
    for (i = 0; i < numUsers; i++) {
        if (users[i].accountNumber == accountNumber) {
            printf("Yeni adınızı girin: ");
            scanf("%s", users[i].name);

            printf("Yeni bakiyenizi girin: ");
            scanf("%lf", &users[i].balance);

            printf("Hesap başarıyla güncellendi.\n");
            return;
        }
    }

    printf("Hesap bulunamadı.\n");
}

void deleteUser() {
    int accountNumber;
    printf("Silinecek hesap numarasını girin: ");
    scanf("%d", &accountNumber);

    int i;
    for (i = 0; i < numUsers; i++) {
        if (users[i].accountNumber == accountNumber) {
            int j;
            for (j = i; j < numUsers - 1; j++) {
                users[j] = users[j + 1];
            }
            numUsers--;

            printf("Hesap başarıyla silindi.\n");
            return;
        }
    }

    printf("Hesap bulunamadı.\n");
}

void deposit() {
    int accountNumber;
    printf("Para yatırılacak hesap numarasını girin: ");
    scanf("%d", &accountNumber);

    double amount;
    printf("Yatırılacak miktarı girin: ");
    scanf("%lf", &amount);

    int i;
    for (i = 0; i < numUsers; i++) {
        if (users[i].accountNumber == accountNumber) {
            users[i].balance += amount;
            printf("Para başarıyla yatırıldı. Güncel bakiye: %.2f\n", users[i].balance);
            return;
        }
    }

    printf("Hesap bulunamadı.\n");
}

void withdraw() {
    int accountNumber;
    printf("Para çekilecek hesap numarasını girin: ");
    scanf("%d", &accountNumber);

    double amount;
    printf("Çekilecek miktarı girin: ");
    scanf("%lf", &amount);

    int i;
    for (i = 0; i < numUsers; i++) {
        if (users[i].accountNumber == accountNumber) {
            if (users[i].balance >= amount) {
                users[i].balance -= amount;
                printf("Para başarıyla çekildi. Güncel bakiye: %.2f\n", users[i].balance);
            } else {
                printf("Yetersiz bakiye.\n");
            }
            return;
        }
    }

    printf("Hesap bulunamadı.\n");
}

void displayBalance() {
    int accountNumber;
    printf("Bakiyesini görüntülemek istediğiniz hesap numarasını girin: ");
    scanf("%d", &accountNumber);

    int i;
    for (i = 0; i < numUsers; i++) {
        if (users[i].accountNumber == accountNumber) {
            printf("Hesap Bakiyesi: %.2f\n", users[i].balance);
            return;
        }
    }

    printf("Hesap bulunamadı.\n");
}

void saveDataToFile() {
    FILE *file = fopen("banka_verileri.txt", "w");
    if (file == NULL) {
        printf("Dosya oluşturma hatası.\n");
        return;
    }

    int i;
    for (i = 0; i < numUsers; i++) {
        fprintf(file, "%s %d %.2f\n", users[i].name, users[i].accountNumber, users[i].balance);
    }

    fclose(file);

    printf("Veriler dosyaya kaydedildi.\n");
}

void loadDataFromFile() {
    FILE *file = fopen("banka_verileri.txt", "r");
    if (file == NULL) {
        printf("Dosya açma hatası.\n");
        return;
    }

    numUsers = 0;

    while (fscanf(file, "%s %d %lf", users[numUsers].name, &users[numUsers].accountNumber, &users[numUsers].balance) == 3) {
        numUsers++;
    }

    fclose(file);

    printf("Veriler dosyadan yüklendi.\n");
}

void printMenu() {
    printf("\n----- BANKA HESAP YÖNETİMİ -----\n");
    printf("1. Yeni Hesap Oluştur\n");
    printf("2. Hesap Güncelle\n");
    printf("3. Hesap Sil\n");
    printf("4. Para Yatır\n");
    printf("5. Para Çek\n");
    printf("6. Bakiye Görüntüle\n");
    printf("7. Verileri Dosyaya Kaydet\n");
    printf("8. Dosyadan Verileri Yükle\n");
    printf("9. Çıkış\n");
    printf("-------------------------------\n");
    printf("Seçiminizi yapın: ");
}

int main() {
    loadDataFromFile();

    int choice;
    do {
        printMenu();
        scanf("%
