---
  title: 'AngularTestFile.ts'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos P'
  ---
  
  
  
  # AngularTestFile.ts
  # Explanation of File Drop Zone Component

This code snippet is an Angular component named `FileDropZoneComponent` that handles file drop functionality. Below is a detailed explanation of the code:

1. **Imports**:
   - `Component`, `EventEmitter`, and `Output` are imported from `@angular/core`. These are essential Angular decorators and classes for defining components and handling data flow.

2. **Component Decorator**:
   - The `@Component` decorator is used to define metadata for the component, including the selector, template URL, and style URLs.

3. **Properties**:
   - `selectedFiles` and `setError` are instances of `EventEmitter` used to emit events when files are selected or errors occur.
   - `allowedExtensions` is an array containing valid file extensions.
   - `maxFiles` specifies the maximum number of files that can be uploaded.

4. **handleDragOver Method**:
   - Prevents the default behavior of the dragover event.

5. **handleDrop Method**:
   - Prevents the default behavior of the drop event.
   - Extracts files from the data transfer items and validates them.

6. **extractFiles Method**:
   - Asynchronously extracts files from the `DataTransferItemList`.
   - Handles both single files and directories by recursively traversing the file tree.
   - Emits an error if an invalid file type is encountered.

7. **traverseFileTree Method**:
   - Recursively traverses the file tree to extract files from directories.

8. **isValidFile Method**:
   - Checks if a file has a valid extension based on the `allowedExtensions` array.

9. **handleFileSelect Method**:
   - Handles file selection from an input element and validates the selected files.

10. **validateAndSetFiles Method**:
    - Filters out invalid files and emits an error if the number of files exceeds the maximum allowed.
    - Emits the selected files or an empty error message.

### Example Usage:
```html
<app-file-drop-zone 
  (selectedFiles)="onFilesSelected($event)" 
  (setError)="onError($event)">
</app-file-drop-zone>
```

In this example, the `FileDropZoneComponent` is used in an Angular template. The component emits `selectedFiles` and `setError` events, which can be handled in the parent component to process the selected files or display error messages.
  
  ## Component Code
  ```jsx
  import { Component, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-file-drop-zone',
  templateUrl: './file-drop-zone.component.html',
  styleUrls: ['./file-drop-zone.component.css']
})
export class FileDropZoneComponent {
  @Output() selectedFiles = new EventEmitter<File[]>();
  @Output() setError = new EventEmitter<string>();

  allowedExtensions = [
    ".js", ".jsx", ".ts", ".tsx", ".py", ".java", ".rb", ".php",
    ".html", ".css", ".cpp", ".c", ".go", ".rs", ".swift", ".kt",
    ".m", ".h", ".cs", ".json", ".xml", ".sh", ".yml", ".yaml",
    ".vue", ".svelte", ".qwik"
  ];
  maxFiles = 4;

  handleDragOver(event: DragEvent) {
    event.preventDefault();
  }

  handleDrop(event: DragEvent) {
    event.preventDefault();
    const items = event.dataTransfer.items;
    this.extractFiles(items).then(files => this.validateAndSetFiles(files));
  }

  async extractFiles(items: DataTransferItemList): Promise<File[]> {
    const filesArray: File[] = [];
    for (let i = 0; i < items.length; i++) {
      const item = items[i].webkitGetAsEntry();
      if (item.isFile) {
        const file = await new Promise<File>((resolve) => item.file(resolve));
        if (this.isValidFile(file)) {
          filesArray.push(file);
        } else {
          this.setError.emit(`Invalid file type. Only ${this.allowedExtensions.join(", ")} files are allowed.`);
        }
      } else if (item.isDirectory) {
        await this.traverseFileTree(item, filesArray);
      }
    }
    return filesArray;
  }

  traverseFileTree(item: any, filesArray: File[]): Promise<void> {
    return new Promise((resolve) => {
      if (item.isFile) {
        item.file((file: File) => {
          if (this.isValidFile(file)) {
            filesArray.push(file);
          }
          resolve();
        });
      } else if (item.isDirectory) {
        const dirReader = item.createReader();
        dirReader.readEntries(async (entries: any[]) => {
          for (const entry of entries) {
            await this.traverseFileTree(entry, filesArray);
          }
          resolve();
        });
      }
    });
  }

  isValidFile(file: File): boolean {
    return this.allowedExtensions.some(ext => file.name.endsWith(ext));
  }

  handleFileSelect(event: Event) {
    const input = event.target as HTMLInputElement;
    const files = Array.from(input.files);
    this.validateAndSetFiles(files);
  }

  validateAndSetFiles(files: File[]) {
    const filteredFiles = files.filter(file => this.isValidFile(file));
    if (filteredFiles.length > this.maxFiles) {
      this.setError.emit(`You can only upload a maximum of ${this.maxFiles} files.`);
    } else {
      this.selectedFiles.emit(filteredFiles.slice(0, this.maxFiles));
      this.setError.emit("");
    }
  }
}
  ```
  