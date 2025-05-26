import java.util.Arrays;
import java.util.Random;

public class SortingComparison {
    
    // Сортировка пузырьком (Bubble Sort)
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Меняем элементы местами
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            // Если не было перестановок, массив уже отсортирован
            if (!swapped) break;
        }
    }
    
    // Быстрая сортировка (Quick Sort)
    public static void quickSort(int[] arr) {
        quickSort(arr, 0, arr.length - 1);
    }
    
    private static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            // Разделение массива
            int pi = partition(arr, low, high);
            
            // Рекурсивная сортировка подмассивов
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }
    
    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                // Меняем элементы местами
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        
        // Меняем pivot с элементом на позиции i+1
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        
        return i + 1;
    }
    
    // Генерация случайного массива
    public static int[] generateRandomArray(int size) {
        Random random = new Random();
        int[] arr = new int[size];
        for (int i = 0; i < size; i++) {
            arr[i] = random.nextInt(10000);
        }
        return arr;
    }
    
    // Копирование массива
    public static int[] copyArray(int[] original) {
        return Arrays.copyOf(original, original.length);
    }
    
    // Измерение времени выполнения
    public static void measureSortingTime(int[] arr, String sortName, Runnable sortFunction) {
        long startTime = System.nanoTime();
        sortFunction.run();
        long endTime = System.nanoTime();
        long duration = (endTime - startTime) / 1_000_000; // в миллисекундах
        
        System.out.printf("%s для массива из %,d элементов: %d мс%n", 
                         sortName, arr.length, duration);
    }
    
    public static void main(String[] args) {
        // Тестирование на небольших массивах (100 элементов)
        int[] smallArray = generateRandomArray(100);
        int[] smallArrayCopy = copyArray(smallArray);
        
        System.out.println("=== Маленький массив (100 элементов) ===");
        measureSortingTime(smallArray, "Bubble Sort", () -> bubbleSort(smallArray));
        measureSortingTime(smallArrayCopy, "Quick Sort", () -> quickSort(smallArrayCopy));
        
        // Тестирование на средних массивах (10,000 элементов)
        int[] mediumArray = generateRandomArray(10_000);
        int[] mediumArrayCopy = copyArray(mediumArray);
        
        System.out.println("\n=== Средний массив (10,000 элементов) ===");
        measureSortingTime(mediumArray, "Bubble Sort", () -> bubbleSort(mediumArray));
        measureSortingTime(mediumArrayCopy, "Quick Sort", () -> quickSort(mediumArrayCopy));
        
        // Тестирование на больших массивах (100,000 элементов)
        int[] largeArray = generateRandomArray(100_000);
        int[] largeArrayCopy = copyArray(largeArray);
        
        System.out.println("\n=== Большой массив (100,000 элементов) ===");
        // Bubble Sort для такого большого массива будет очень медленным, поэтому пропустим
        measureSortingTime(largeArrayCopy, "Quick Sort", () -> quickSort(largeArrayCopy));
    }
}
