# -Implementa-o-de-Algoritmos-de-Ordena-o-em-C

#include <stdio.h>
#include <stdlib.h>
#include <time.h>

/*
=========================================================
TRABALHO: IMPLEMENTAÇÃO DE ALGORITMOS DE ORDENAÇÃO
Algoritmos implementados:
- Bubble Sort
- Selection Sort
- Insertion Sort
- Quick Sort
- Merge Sort

O programa permite comparar o desempenho utilizando
medição de tempo com clock().
=========================================================
*/


// =========================================================
// Função para imprimir os elementos do array
// =========================================================
void printArray(int arr[], int size) {
    for(int i = 0; i < size; i++)
        printf("%d ", arr[i]);
    printf("\n");
}


// =========================================================
// Função para copiar um array para outro
// Garante que todos os algoritmos ordenem o mesmo vetor
// =========================================================
void copyArray(int source[], int dest[], int size) {
    for(int i = 0; i < size; i++)
        dest[i] = source[i];
}


// =========================================================
// BUBBLE SORT
// Compara elementos adjacentes e os troca se necessário.
// Complexidade média: O(n²)
// =========================================================
void bubbleSort(int arr[], int size) {
    for(int i = 0; i < size - 1; i++) {
        for(int j = 0; j < size - i - 1; j++) {
            if(arr[j] > arr[j+1]) {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}


// =========================================================
// SELECTION SORT
// Seleciona o menor elemento e o posiciona corretamente.
// Complexidade: O(n²)
// =========================================================
void selectionSort(int arr[], int size) {
    for(int i = 0; i < size - 1; i++) {
        int min = i;

        for(int j = i + 1; j < size; j++) {
            if(arr[j] < arr[min])
                min = j;
        }

        // Troca o menor encontrado com a posição atual
        int temp = arr[i];
        arr[i] = arr[min];
        arr[min] = temp;
    }
}


// =========================================================
// INSERTION SORT
// Insere cada elemento na posição correta.
// Melhor caso: O(n)
// Pior caso: O(n²)
// =========================================================
void insertionSort(int arr[], int size) {
    for(int i = 1; i < size; i++) {
        int key = arr[i];
        int j = i - 1;

        // Move elementos maiores para frente
        while(j >= 0 && arr[j] > key) {
            arr[j+1] = arr[j];
            j--;
        }

        arr[j+1] = key;
    }
}


// =========================================================
// QUICK SORT
// Utiliza divisão e conquista.
// Escolhe um pivô e particiona o vetor.
// Complexidade média: O(n log n)
// =========================================================

// Função para particionar o vetor
int partition(int arr[], int low, int high) {

    int pivot = arr[high];   // Define o último elemento como pivô
    int i = low - 1;         // Índice do menor elemento

    for(int j = low; j < high; j++) {

        if(arr[j] < pivot) {
            i++;

            // Troca arr[i] com arr[j]
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }

    // Coloca o pivô na posição correta
    int temp = arr[i+1];
    arr[i+1] = arr[high];
    arr[high] = temp;

    return i + 1;
}

// Função recursiva do Quick Sort
void quickSort(int arr[], int low, int high) {
    if(low < high) {
        int pi = partition(arr, low, high);

        // Ordena as duas partições
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}


// =========================================================
// MERGE SORT
// Divide o vetor até restarem subvetores unitários
// e depois realiza a intercalação ordenada.
// Complexidade: O(n log n)
// =========================================================

// Função para intercalar dois subvetores
void merge(int arr[], int left, int mid, int right) {

    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Vetores temporários
    int L[n1], R[n2];

    // Copiando dados para vetores auxiliares
    for(int i = 0; i < n1; i++)
        L[i] = arr[left + i];

    for(int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    int i = 0, j = 0, k = left;

    // Intercala os vetores temporários
    while(i < n1 && j < n2) {
        if(L[i] <= R[j])
            arr[k++] = L[i++];
        else
            arr[k++] = R[j++];
    }

    // Copia elementos restantes
    while(i < n1)
        arr[k++] = L[i++];

    while(j < n2)
        arr[k++] = R[j++];
}


// Função recursiva do Merge Sort
void mergeSort(int arr[], int left, int right) {
    if(left < right) {

        int mid = left + (right - left) / 2;

        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        merge(arr, left, mid, right);
    }
}


// =========================================================
// Gera um vetor com números aleatórios
// =========================================================
void generateRandomArray(int arr[], int size) {
    for(int i = 0; i < size; i++)
        arr[i] = rand() % 10000;
}


// =========================================================
// FUNÇÃO PRINCIPAL
// =========================================================
int main() {

    int size, option;

    printf("Digite o tamanho do array: ");
    scanf("%d", &size);

    int original[size];
    int copy[size];

    srand(time(NULL));  // Inicializa a semente do rand()

    generateRandomArray(original, size);

    printf("\nArray original:\n");
    printArray(original, size);

    printf("\nEscolha o algoritmo:\n");
    printf("1 - Bubble Sort\n");
    printf("2 - Selection Sort\n");
    printf("3 - Insertion Sort\n");
    printf("4 - Quick Sort\n");
    printf("5 - Merge Sort\n");
    printf("6 - Comparar Todos\n");
    scanf("%d", &option);

    clock_t start, end;
    double time_spent;

    switch(option) {

        case 1:
            copyArray(original, copy, size);
            start = clock();
            bubbleSort(copy, size);
            end = clock();
            break;

        case 2:
            copyArray(original, copy, size);
            start = clock();
            selectionSort(copy, size);
            end = clock();
            break;

        case 3:
            copyArray(original, copy, size);
            start = clock();
            insertionSort(copy, size);
            end = clock();
            break;

        case 4:
            copyArray(original, copy, size);
            start = clock();
            quickSort(copy, 0, size - 1);
            end = clock();
            break;

        case 5:
            copyArray(original, copy, size);
            start = clock();
            mergeSort(copy, 0, size - 1);
            end = clock();
            break;

        case 6:
            printf("\nComparando algoritmos:\n");

            copyArray(original, copy, size);
            start = clock();
            bubbleSort(copy, size);
            end = clock();
            printf("Bubble Sort: %f s\n", ((double)(end-start))/CLOCKS_PER_SEC);

            copyArray(original, copy, size);
            start = clock();
            selectionSort(copy, size);
            end = clock();
            printf("Selection Sort: %f s\n", ((double)(end-start))/CLOCKS_PER_SEC);

            copyArray(original, copy, size);
            start = clock();
            insertionSort(copy, size);
            end = clock();
            printf("Insertion Sort: %f s\n", ((double)(end-start))/CLOCKS_PER_SEC);

            copyArray(original, copy, size);
            start = clock();
            quickSort(copy, 0, size - 1);
            end = clock();
            printf("Quick Sort: %f s\n", ((double)(end-start))/CLOCKS_PER_SEC);

            copyArray(original, copy, size);
            start = clock();
            mergeSort(copy, 0, size - 1);
            end = clock();
            printf("Merge Sort: %f s\n", ((double)(end-start))/CLOCKS_PER_SEC);

            return 0;

        default:
            printf("Opcao invalida!\n");
            return 0;
    }

    time_spent = ((double)(end - start)) / CLOCKS_PER_SEC;

    printf("\nArray ordenado:\n");
    printArray(copy, size);
    printf("Tempo: %f segundos\n", time_spent);

    return 0;
}
