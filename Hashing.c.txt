
#include <stdio.h>

#define SIZE 61

struct Product {
    int id;
    char name[30];
    int occupied;
};

struct Product hashTable[SIZE];

int totalCollisions = 0;
int insertionComparisons = 0;
int searchComparisons = 0;

int hashFunction(int id) {
    return id % SIZE;
}

void insert(int id, char name[]) {
    int index = hashFunction(id);
    int start = index;

    insertionComparisons = 0;

    while (hashTable[index].occupied) {
        insertionComparisons++;
        totalCollisions++;

        if (hashTable[index].id == id) {
            printf("Duplicate Product ID!\n");
            totalCollisions--;
            return;
        }

        index = (index + 1) % SIZE;

        if (index == start) {
            printf("Hash Table is Full!\n");
            return;
        }
    }

    hashTable[index].id = id;

    int i = 0;
    while (name[i] != '\0' && i < 29) {
        hashTable[index].name[i] = name[i];
        i++;
    }
    hashTable[index].name[i] = '\0';

    hashTable[index].occupied = 1;
}

int search(int id) {
    int index = hashFunction(id);
    int start = index;

    searchComparisons = 0;

    while (hashTable[index].occupied) {
        searchComparisons++;

        if (hashTable[index].id == id)
            return index;

        index = (index + 1) % SIZE;

        if (index == start)
            break;
    }

    return -1;
}

int main() {
    int ids[] = {
        105,102,108,101,110,103,107,104,112,106,
        115,109,120,111,118,113,125,114,122,116,
        130,117,128,119,135,121,132,123,140,124
    };

    char *names[] = {
        "Laptop","Mouse","Keyboard","Monitor","Printer",
        "Headphones","Webcam","Speaker","Tablet","Smartphone",
        "Router","Scanner","Microphone","Projector","HardDisk",
        "Pendrive","SSD","GraphicsCard","Motherboard","Processor",
        "RAM","PowerSupply","UPS","CoolingFan","PrinterInk",
        "MonitorStand","LaptopBag","USBHub","EthernetCable",
        "WebcamStand"
    };

    int i, key, result;

    for (i = 0; i < SIZE; i++)
        hashTable[i].occupied = 0;

    for (i = 0; i < 30; i++)
        insert(ids[i], names[i]);

    printf("===== PRODUCT SEARCH USING HASHING =====\n");

    printf("\nHASH TABLE RECORDS:\n");

    for (i = 0; i < SIZE; i++) {
        if (hashTable[i].occupied) {
            printf("Index %d: ID %d - %s\n",
                   i, hashTable[i].id,
                   hashTable[i].name);
        }
    }

    printf("\nTotal Insertion Collisions: %d\n",
           totalCollisions);

    printf("Insertion Comparisons: %d\n",
           insertionComparisons);

    printf("\nSUCCESSFUL SEARCH (ID 115):\n");

    result = search(115);

    if (result != -1)
        printf("Product Found: %s\n",
               hashTable[result].name);
    else
        printf("Product Not Found\n");

    printf("Search Comparisons: %d\n",
           searchComparisons);

    printf("\nUNSUCCESSFUL SEARCH (ID 999):\n");

    result = search(999);

    if (result != -1)
        printf("Product Found: %s\n",
               hashTable[result].name);
    else
        printf("Product Not Found\n");

    printf("Search Comparisons: %d\n",
           searchComparisons);

    printf("\nEnter a Product ID to search: ");
    scanf("%d", &key);

    result = search(key);

    if (result != -1)
        printf("Product Found: %s\n",
               hashTable[result].name);
    else
        printf("Product Not Found\n");

    printf("Search Comparisons: %d\n",
           searchComparisons);

    return 0;
}
