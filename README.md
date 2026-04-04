# 排序報告

學號: 11428142  
姓名: 李恩愷  
模擬頁面: [https://allen297.github.io/sort_report/](https://allen297.github.io/sort_report/)

## 氣泡排序法 (Bubble Sort)
**簡述原理：**
氣泡排序法會重複走訪要排序的數列，一次比較兩個相鄰的元素，如果順序錯誤就將他們交換。走訪數列的工作會重複進行，直到沒有再需要交換的元素為止。越大的元素會像氣泡一樣慢慢「浮」到數列的頂端（最後面）。

**複雜度分析：**
* 時間複雜度：
  * 最佳情況：O(n) （當資料已經是排序好的狀態）
  * 平均/最壞情況：O(n^2)
* 空間複雜度：O(1) （只需一個額外空間進行交換）

**模擬實作內容：**
```cpp
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```

---
## 選擇排序法 (Selection Sort)
**簡述原理：**
將數列分為「已排序」與「未排序」兩部分。每次從「未排序」的數列中找出最小值，並將它與未排序區域的第一個元素交換，將其納入「已排序」區域。重複此動作直到所有元素皆排序完畢。

**複雜度分析：**
* 時間複雜度：最佳、平均、最壞情況皆為 O(n^2) （無論資料是否已排序，都需要尋找最小值）
* 空間複雜度：O(1)

**模擬實作內容：**
```cpp
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        swap(arr[i], arr[min_idx]);
    }
}
```

---
## 插入排序法 (Insertion Sort)
**簡述原理：**
類似我們打撲克牌時整理手牌的方式。將數列的第一個元素視為已排序，接著每次取出下一個未排序元素，在已排序的序列中由後往前掃描，找到合適的位置並將其插入。

**複雜度分析：**
* 時間複雜度：
  * 最佳情況：O(n) （當資料已經是排序好的狀態）
  * 平均/最壞情況：O(n^2)
* 空間複雜度：O(1)

**模擬實作內容：**
```cpp
void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        // 將大於 key 的元素往後移
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
```

---
## 合併排序法 (Merge Sort)
**簡述原理：**
採用「分而治之 (Divide and Conquer)」策略。將未排序數列從中間切半，遞迴地對這兩個子數列進行排序，最後將排好序的兩個子數列「合併」成一個單一的、已排序的數列。

**複雜度分析：**
* 時間複雜度：最佳、平均、最壞情況皆為 O(n log n)
* 空間複雜度：O(n) （合併時需要額外的陣列空間來暫存資料）

**模擬實作內容：**
```cpp
void merge(int arr[], int l, int m, int r) {
    int n1 = m - l + 1, n2 = r - m;
    int L[n1], R[n2];
    for (int i = 0; i < n1; i++) L[i] = arr[l + i];
    for (int j = 0; j < n2; j++) R[j] = arr[m + 1 + j];
    
    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}
```

---
## 堆積排序法 (Heap Sort)
**簡述原理：**
利用「二元堆積 (Binary Heap)」資料結構來進行排序。首先將整個陣列建構成一個「最大堆積樹 (Max Heap)」，此時陣列的最大值會在樹的根節點。接著將根節點與陣列最後一個元素交換，並縮小堆積的範圍，重新調整剩下的元素為最大堆積。重複此步驟直到所有元素排序完成。

**複雜度分析：**
* 時間複雜度：最佳、平均、最壞情況皆為 O(n log n)
* 空間複雜度：O(1) （屬於原地排序演算法，不需要額外大陣列）

**模擬實作內容：**
```cpp
void heapify(int arr[], int n, int i) {
    int largest = i; 
    int left = 2 * i + 1; 
    int right = 2 * i + 2; 

    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    // 建立最大堆積樹
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // 一個個從堆積中取出元素
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

---
## 綜合比較

| 排序演算法 | 平均時間複雜度 | 最壞時間複雜度 | 空間複雜度 | 穩定性 |
| :--- | :--- | :--- | :--- | :--- |
| **氣泡排序** | O(n^2) | O(n^2) | O(1) | 穩定 |
| **選擇排序** | O(n^2) | O(n^2) | O(1) | 不穩定 |
| **插入排序** | O(n^2) | O(n^2) | O(1) | 穩定 |
| **合併排序** | O(n log n) | O(n log n) | O(n) | 穩定 |
| **堆積排序** | O(n log n) | O(n log n) | O(1) | 不穩定 |

---
## 學習心得
  在實作這五種排序法與架設 GitHub 模擬網頁的過程中，我獲得了許多實際且深刻的體會。透過親自生成氣泡排序、選擇排序、插入排序、合併排序與堆積排序，我更清楚理解不同演算法在運作上的差異，以及它們在效率上的表現。特別是在處理較大資料量時，可以明顯感受到各種排序方法之間的效能落差，讓我對時間複雜度不再只是理論上的概念，而是有了具體的感受。
此外，在架設 GitHub 模擬網頁的過程中，我學習到如何將程式成果進行展示，並理解基本的網頁架設流程。從整理檔案、上傳專案到成功呈現頁面，讓我對專案整體的完成流程有更完整的認識。

  整體而言，這次的實作不僅加深了我對排序演算法的理解，也提升了我在程式設計、除錯以及系統整合方面的能力，對未來進一步學習資料結構與軟體開發有很大的幫助。

