##### `Tldr: cp source_file destination_path
---
### Copy a file to a new place

```bash
cp source_file destination_path
```

#### 1. **Copy and rename at the same time**

```bash
cp ~/Documents/file.txt ~/Desktop/file_copy.txt
```

#### 2. **With overwrite confirmation (interactive mode)**

```bash
cp -i file.txt ~/backup/
```

#### 3. **Show progress and verbose output**

```bash
cp -v file.txt ~/Desktop/
```

This shows messages like:

```
'file.txt' -> '/home/potaxo/Desktop/file.txt'
```

---
### ⚠️ Notes:

* If the destination is a **directory**, the file keeps its original name.
* If the destination **does not exist**, the file will be **renamed** to that name.
* ==Use **quotes** if the file path contains **spaces**.==

```bash
cp "my file.txt" "/home/potaxo/Backups/"
```
---
Relevant Link: [[Linux]]
Date: 2025-09-23 
Time: 20:41