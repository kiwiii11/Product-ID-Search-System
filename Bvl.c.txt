
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Product {
    int id;
    char name[30];
    struct Product *left, *right;
};

int insertionComparisons = 0;
int searchComparisons = 0;

struct Product* createNode(int id, char name[]) {
    struct Product *newNode =
        (struct Product*)malloc(sizeof(struct Product));

    newNode->id = id;
    strcpy(newNode->name, name);
    newNode->left = NULL;
    newNode->right = NULL;

    return newNode;
}

struct Product* insert(struct Product *root, int id, char name[]) {
    if (root == NULL)
        return createNode(id, name);

    insertionComparisons++;

    if (id < root->id)
        root->left = insert(root->left, id, name);
    else if (id > root->id)
        root->right = insert(root->right, id, name);

    return root;
}

struct Product* search(struct Product *root, int id) {
    if (root == NULL)
        return NULL;

    searchComparisons++;

    if (root->id == id)
        return root;

    if (id < root->id)
        return search(root->left, id);

    return search(root->right, id);
}

int height(struct Product *root) {
    if (root == NULL)
        return 0;

    int leftHeight = height(root->left);
    int rightHeight = height(root->right);

    return 1 + (leftHeight > rightHeight ?
                leftHeight : rightHeight);
}

void inorder(struct Product *root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d - %s\n", root->id, root->name);
        inorder(root->right);
    }
}

void preorder(struct Product *root) {
    if (root != NULL) {
        printf("%d - %s\n", root->id, root->name);
        preorder(root->left);
        preorder(root->right);
    }
}

void postorder(struct Product *root) {
    if (root != NULL) {
        postorder(root->left);
        postorder(root->right);
        printf("%d - %s\n", root->id, root->name);
    }
}

int main() {
    struct Product *root = NULL;

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

    int i, key;
    struct Product *result;

    for (i = 0; i < 30; i++)
        root = insert(root, ids[i], names[i]);

    printf("===== PRODUCT SEARCH USING BST =====\n");

    printf("\nINORDER TRAVERSAL:\n");
    inorder(root);

    printf("\nPREORDER TRAVERSAL:\n");
    preorder(root);

    printf("\nPOSTORDER TRAVERSAL:\n");
    postorder(root);

    printf("\nTree Height: %d\n", height(root));
    printf("Insertion Comparisons: %d\n",
           insertionComparisons);

    printf("\nSUCCESSFUL SEARCH (ID 115):\n");
    searchComparisons = 0;
    result = search(root, 115);

    if (result != NULL)
        printf("Product Found: %s\n", result->name);
    else
        printf("Product Not Found\n");

    printf("Search Comparisons: %d\n", searchComparisons);

    printf("\nUNSUCCESSFUL SEARCH (ID 999):\n");
    searchComparisons = 0;
    result = search(root, 999);

    if (result != NULL)
        printf("Product Found: %s\n", result->name);
    else
        printf("Product Not Found\n");

    printf("Search Comparisons: %d\n", searchComparisons);

    printf("\nEnter a Product ID to search: ");
    scanf("%d", &key);

    searchComparisons = 0;
    result = search(root, key);

    if (result != NULL)
        printf("Product Found: %s\n", result->name);
    else
        printf("Product Not Found\n");

    printf("Search Comparisons: %d\n", searchComparisons);

    return 0;
}
