/*
 Created by Kevin Chavez
Instructor: Juan Rodriguez
Class: CMPS 400 Analysis of Algorithms
Assignment #1
Problem #1
 */
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <cstring>
#include <climits>
using namespace std;

struct SearchResult {
    int index;       // index of first match, or -1 if not found
    int comparisons; // number of comparisons made
    bool foundResult;
};
struct SortResult {
    int comparisons; // number of value comparisons
    int dataMoves;   // swaps for selection sort, shifts/inserts for insertion sort
};
struct Dataset {
    int id;            // auto-generated unique ID
    int values[100];   // current working copy
    int original[100]; // original copy for restore
    int size;          // number of elements
    bool isSorted;     // true if values[] is currently sorted ascending
};
// Track all IDs used this run to avoid duplicates
int usedIDs[10000];
int usedIDCount = 0;
// generateUniqueID: Creates a unique dataset ID in range
//                  10000?99999, avoiding repeats this run.
int generateUniqueID() {
    int id;
    bool duplicate;
    do {
        duplicate = false;
        id = 10000 + rand() % 90000;
        for (int i = 0; i < usedIDCount; i++) {
            if (usedIDs[i] == id) { duplicate = true; break; }
        }
    } while (duplicate);
    usedIDs[usedIDCount++] = id;
    return id;
}
// checkSorted: Returns true if arr[] is sorted ascending.
bool checkSorted(const int arr[], int n) {
    for (int i = 0; i < n - 1; i++)
        if (arr[i] > arr[i + 1]) return false;
    return true;
}
// copyArray: Copies n elements from src[] into dest[].
void copyArray(int dest[], const int src[], int n) {
    for (int i = 0; i < n; i++)
        dest[i] = src[i];
}
// displayArray: Prints all n elements of arr[] on one line.
void displayArray(const int arr[], int n) {
    for (int i = 0; i < n; i++) {
        cout << arr[i];
        if (i < n - 1) cout << " ";
    }
    cout << endl;
}
// linearSearch: Scans arr[] left-to-right for target.
//               Returns first index found or -1.
SearchResult linearSearch(const int arr[], int n, int target) {
    SearchResult result = {-1, 0, false};
    for (int i = 0; i < n; i++) {
        result.comparisons++;
        if (arr[i] == target) {
            result.index = i;
            result.foundResult = true;
            break;
        }
    }
    return result;
}
// binarySearch: Iterative binary search on a sorted arr[].
//               Returns index of target or -1.
SearchResult binarySearch(const int arr[], int n, int target) {
    SearchResult result = {-1, 0, false};
    int low = 0, high = n - 1;
    while (low <= high) {
        result.comparisons++;
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) {
            result.index = mid;
            result.foundResult = true;
            break;
        } else if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return result;
}
// selectionSort: Sorts arr[] ascending using Selection Sort.
//                Counts comparisons and swaps.
SortResult selectionSort(int arr[], int n) {
    SortResult result = {0, 0};
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            result.comparisons++;
            if (arr[j] < arr[minIdx])
                minIdx = j;
        }
        if (minIdx != i) {
            int tmp = arr[i];
            arr[i] = arr[minIdx];
            arr[minIdx] = tmp;
            result.dataMoves++;
        }
    }
    return result;
}
// insertionSort: Sorts arr[] ascending using Insertion Sort.
// Counts comparisons and data moves (shifts).
SortResult insertionSort(int arr[], int n) {
    SortResult result = {0, 0};
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0) {
            result.comparisons++;
            if (arr[j] > key) {
                arr[j + 1] = arr[j];
                result.dataMoves++;
                j--;
            } else {
                break;
            }
        }
        arr[j + 1] = key;
        if (j + 1 != i) result.dataMoves++; // count the final insert as a move
    }
    return result;
}
// pauseForUser: Waits for the user to press Enter.
void pauseForUser() {
    cout << "\nPress Enter to return to Main Menu!";
    cin.ignore(INT_MAX, '\n');
    cin.get();
}
// printHeader: Displays the application banner.
void printHeader() {
    cout << "\n*************** Algorithm Workbench ***************\n";
    cout << "Type a number to choose an action:\n\n";
    cout << "1- Create a new dataset\n";
    cout << "2- Display current dataset\n";
    cout << "3- Perform Linear Search\n";
    cout << "4- Perform Binary Search\n";
    cout << "5- Sort using Selection Sort\n";
    cout << "6- Sort using Insertion Sort\n";
    cout << "7- Restore original dataset\n";
    cout << "0- Exit\n";
    cout << "---------------------------------------------------\n";
    cout << "Choice: ";
}
// option1: Create a new dataset (manual or random).
void option_1(Dataset &ds, bool &hasDataset) {
    cout << "\n--- Create a New Dataset ---\n";
    int size;
    do {
        cout << "Enter dataset size (1-100): ";
        cin >> size;
        if (size < 1 || size > 100)
            cout << "Invalid size. Please enter a value between 1 and 100.\n";
    } while (size < 1 || size > 100);

    cout << "\nChoose input method:\n";
    cout << "1- Manual\n";
    cout << "2- Random\n";
    cout << "Choice: ";
    int method;
    cin >> method;

    if (method == 1) {
        // Manual entry
        for (int i = 0; i < size; i++) {
            cout << "Enter value " << (i + 1) << ": ";
            cin >> ds.values[i];
        }
    } else {
        // Random generation
        int minVal, maxVal;
        cout << "Enter minimum value: ";
        cin >> minVal;
        cout << "Enter maximum value: ";
        cin >> maxVal;
        if (minVal > maxVal) swap(minVal, maxVal);
        int range = maxVal - minVal + 1;
        for (int i = 0; i < size; i++)
            ds.values[i] = minVal + rand() % range;
    }

    ds.size = size;
    ds.id = generateUniqueID();
    copyArray(ds.original, ds.values, size);
    ds.isSorted = checkSorted(ds.values, size);
    hasDataset = true;

    cout << "\nGreat! A new dataset has been created.\n";
    cout << "Dataset ID: " << ds.id << "\n";
    cout << "Size:       " << ds.size << "\n";
    cout << "Sorted:     " << (ds.isSorted ? "Yes" : "No") << "\n";
    cout << "Values:     ";
    displayArray(ds.values, ds.size);
    pauseForUser();
}
// option2: Display the current dataset.
void option_2(const Dataset &ds, bool hasDataset) {
    cout << "\n--- Display Current Dataset ---\n";
    if (!hasDataset) {
        cout << "No dataset has been created yet.\n";
    } else {
        cout << "Dataset ID: " << ds.id << "\n";
        cout << "Size:       " << ds.size << "\n";
        cout << "Sorted:     " << (ds.isSorted ? "Yes" : "No") << "\n";
        cout << "Values:     ";
        displayArray(ds.values, ds.size);
    }
    pauseForUser();
}
// option3: Perform Linear Search on the current dataset.
void option_3(const Dataset &ds, bool hasDataset) {
    cout << "\n--- Linear Search ---\n";
    if (!hasDataset) {
        cout << "No dataset has been created yet.\n";
        pauseForUser();
        return;
    }
    int target;
    cout << "Enter target value: ";
    cin >> target;

    SearchResult sr = linearSearch(ds.values, ds.size, target);

    cout << "\n------ Linear Search Report ------\n";
    cout << "Target:      " << target << "\n";
    cout << "Found:       " << (sr.foundResult ? "Yes" : "No") << "\n";
    cout << "Index:       " << sr.index << "\n";
    cout << "Comparisons: " << sr.comparisons << "\n";
    cout << "\nTheoretical Complexity:\n";
    cout << "  Best Case:    O(1)\n";
    cout << "  Average Case: O(n)\n";
    cout << "  Worst Case:   O(n)\n";
    cout << "----------------------------------\n";
    pauseForUser();
}
// option4: Perform Binary Search (requires sorted dataset).
void option_4(const Dataset &ds, bool hasDataset) {
    cout << "\n--- Binary Search ---\n";
    if (!hasDataset) {
        cout << "No dataset has been created yet.\n";
        pauseForUser();
        return;
    }
    if (!ds.isSorted) {
        cout << "Binary Search cannot run because the dataset is not sorted.\n";
        pauseForUser();
        return;
    }
    int target;
    cout << "Enter target value: ";
    cin >> target;

    SearchResult sr = binarySearch(ds.values, ds.size, target);

    cout << "\n------ Binary Search Report ------\n";
    cout << "Target:      " << target << "\n";
    cout << "Found:       " << (sr.foundResult ? "Yes" : "No") << "\n";
    cout << "Index:       " << sr.index << "\n";
    cout << "Comparisons: " << sr.comparisons << "\n";
    cout << "\nTheoretical Complexity:\n";
    cout << "  Best Case:    O(1)\n";
    cout << "  Average Case: O(log n)\n";
    cout << "  Worst Case:   O(log n)\n";
    cout << "----------------------------------\n";
    pauseForUser();
}
// option5: Sort the dataset using Selection Sort.
void option_5(Dataset &ds, bool hasDataset) {
    cout << "\n--- Selection Sort ---\n";
    if (!hasDataset) {
        cout << "No dataset has been created yet.\n";
        pauseForUser();
        return;
    }
    cout << "Before Selection Sort:\n";
    displayArray(ds.values, ds.size);

    SortResult sr = selectionSort(ds.values, ds.size);
    ds.isSorted = true;

    cout << "After Selection Sort:\n";
    displayArray(ds.values, ds.size);
    cout << "\nComparisons: " << sr.comparisons << "\n";
    cout << "Swaps:       " << sr.dataMoves << "\n";
    cout << "\nTheoretical Complexity:\n";
    cout << "  Time Complexity: O(n^2)\n";
    cout << "  Auxiliary Space: O(1)\n";
    pauseForUser();
}
// option6: Sort the dataset using Insertion Sort.
void option_6(Dataset &ds, bool hasDataset) {
    cout << "\n--- Insertion Sort ---\n";
    if (!hasDataset) {
        cout << "No dataset has been created yet.\n";
        pauseForUser();
        return;
    }
    cout << "Before Insertion Sort:\n";
    displayArray(ds.values, ds.size);

    SortResult sr = insertionSort(ds.values, ds.size);
    ds.isSorted = true;

    cout << "After Insertion Sort:\n";
    displayArray(ds.values, ds.size);
    cout << "\nComparisons: " << sr.comparisons << "\n";
    cout << "Data Moves:  " << sr.dataMoves << "\n";
    cout << "\nTheoretical Complexity:\n";
    cout << "  Best Case:    O(n)\n";
    cout << "  Average Case: O(n^2)\n";
    cout << "  Worst Case:   O(n^2)\n";
    cout << "  Auxiliary Space: O(1)\n";
    pauseForUser();
}
// option7: Restore original dataset from backup copy.
void option_7(Dataset &ds, bool hasDataset) {
    cout << "\n--- Restore Original Dataset ---\n";
    if (!hasDataset) {
        cout << "No dataset has been created yet.\n";
        pauseForUser();
        return;
    }
    copyArray(ds.values, ds.original, ds.size);
    ds.isSorted = checkSorted(ds.values, ds.size);

    cout << "Dataset ID: " << ds.id << "\n";
    cout << "Restored:   ";
    displayArray(ds.values, ds.size);
    cout << "Sorted:     " << (ds.isSorted ? "Yes" : "No") << "\n";
    cout << "\nOriginal dataset restored successfully!\n";
    pauseForUser();
}

int main() {
    srand(static_cast<unsigned>(time(0)));

    Dataset ds;
    bool hasDataset = false;
    int choice;

    do {
        printHeader();
        cin >> choice;

        switch (choice) {
            case 1: option_1(ds, hasDataset); break;
            case 2: option_2(ds, hasDataset); break;
            case 3: option_3(ds, hasDataset); break;
            case 4: option_4(ds, hasDataset); break;
            case 5: option_5(ds, hasDataset); break;
            case 6: option_6(ds, hasDataset); break;
            case 7: option_7(ds, hasDataset); break;
            case 0:
                cout << "\nThank you for using the Algorithm Workbench!\n";
                cout << "Goodbye!\n\n";
                break;
            default:
                cout << "Invalid choice. Please enter 0-7.\n";
                pauseForUser();
                break;
        }
    } while (choice != 0);

    return 0;
}
