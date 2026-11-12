
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Product {
    int id;
    char name[30];
    int height;
    struct Product *left, *right;
};

int insertionComparisons = 0;
int searchComparisons = 0;

int max(int a, int b) {
    return (a > b) ? a : b;
}

int getHeight(struct Product *node) {
    if (node == NULL)
        return 0;
    return node->height;
}

struct Product* createNode(int id, char name[]) {
    struct Product *newNode =
        (struct Product*)malloc(sizeof(struct Product));

    newNode->id = id;
    strcpy(newNode->name, name);
    newNode->left = NULL;
    newNode->right = NULL;
    newNode->height = 1;

    return newNode;
}

struct Product* rightRotate(struct Product *y) {
    struct Product *x = y->left;
    struct Product *temp = x->right;

    x->right = y;
    y->left = temp;

    y->height = 1 + max(getHeight(y->left),
                        getHeight(y->right));

    x->height = 1 + max(getHeight(x->left),
                        getHeight(x->right));

    return x;
}

struct Product* leftRotate(struct Product *x) {
    struct Product *y = x->right;
    struct Product *temp = y->left;

    y->left = x;
    x->right = temp;

    x->height = 1 + max(getHeight(x->left),
                        getHeight(x->right));

    y->height = 1 + max(getHeight(y->left),
                        getHeight(y->right));

    return y;
}

int getBalance(struct Product *node) {
    if (node == NULL)
        return 0;

    return getHeight(node->left) -
           getHeight(node->right);
}

struct Product* insert(struct Product *node,
                       int id, char name[]) {

    if (node == NULL)
        return createNode(id, name);

    insertionComparisons++;

    if (id < node->id)
        node->left = insert(node->left, id, name);

    else if (id > node->id)
        node->right = insert(node->right, id, name);

    else
        return node;

    node->height = 1 + max(getHeight(node->left),
                           getHeight(node->right));

    int balance = getBalance(node);

    // LL Case
    if (balance > 1 && id < node->left->id)
        return rightRotate(node);

    // RR Case
    if (balance < -1 && id > node->right->id)
        return leftRotate(node);

    // LR Case
    if (balance > 1 && id > node->left->id) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    // RL Case
    if (balance < -1 && id < node->right->id) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;
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

    printf("===== PRODUCT SEARCH USING AVL TREE =====\n");

    printf("\nINORDER TRAVERSAL:\n");
    inorder(root);

    printf("\nPREORDER TRAVERSAL:\n");
    preorder(root);

    printf("\nPOSTORDER TRAVERSAL:\n");
    postorder(root);

    printf("\nTree Height: %d\n", getHeight(root));
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
