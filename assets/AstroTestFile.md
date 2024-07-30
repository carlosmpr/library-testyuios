---
  title: 'AstroTestFile.astro'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos P'
  ---
  
  
  
  # AstroTestFile.astro
  ## FileDropZone Component Explanation

The provided code is a React functional component called `FileDropZone`. This component allows users to drag and drop files or folders into a designated area or click to select files for upload. It enforces restrictions on the types of files that can be uploaded and limits the number of files that can be selected.

### Code Explanation

- **useState**: The `useState` hook from React is used to manage the component's state. It is used to store the selected files and any error messages that may occur during file selection.

- **allowedExtensions**: An array containing a list of file extensions that are allowed for upload.

- **maxFiles**: A constant defining the maximum number of files that can be uploaded.

- **handleDragOver**: A function that prevents the default behavior when an item is dragged over the drop zone.

- **handleDrop**: An asynchronous function that handles the drop event when files are dropped into the drop zone. It extracts the files from the data transfer items and validates them before setting the selected files.

- **extractFiles**: An asynchronous function that extracts files from the data transfer items. It checks if the file is valid based on the allowed extensions and handles both files and directories.

- **traverseFileTree**: A function that recursively traverses a file tree to extract files. It handles both files and directories within the tree.

- **isValidFile**: A function that checks if a file is valid based on its extension.

- **handleFileSelect**: A function that handles the file selection event when files are selected using the file input.

- **validateAndSetFiles**: A function that validates the selected files based on the allowed extensions and maximum file limit before setting them in the component state.

### Usage Example

To use the `FileDropZone` component in a React application, you can simply import and render it:

```jsx
import React from 'react';
import FileDropZone from './FileDropZone';

const App = () => {
  return (
    <div>
      <h1>Upload Files</h1>
      <FileDropZone />
    </div>
  );
};

export default App;
```

In this example, the `FileDropZone` component will be rendered within the `App` component, allowing users to drag and drop or select files for upload.
  
  ## Component Code
  ```jsx
  ---
import { useState } from 'react';
import { DocumentTextIcon, ArrowUpTrayIcon } from '@heroicons/react/24/outline';

const allowedExtensions = [
  ".js", ".jsx", ".ts", ".tsx", ".py", ".java", ".rb", ".php",
  ".html", ".css", ".cpp", ".c", ".go", ".rs", ".swift", ".kt",
  ".m", ".h", ".cs", ".json", ".xml", ".sh", ".yml", ".yaml",
  ".vue", ".svelte", ".qwik"
];
const maxFiles = 4;

const FileDropZone = () => {
  const [selectedFiles, setSelectedFiles] = useState([]);
  const [error, setError] = useState('');

  const handleDragOver = (e) => {
    e.preventDefault();
  };

  const handleDrop = async (e) => {
    e.preventDefault();
    const items = e.dataTransfer.items;
    const filesArray = await extractFiles(items);
    validateAndSetFiles(filesArray);
  };

  const extractFiles = async (items) => {
    const filesArray = [];
    for (let i = 0; i < items.length; i++) {
      const item = items[i].webkitGetAsEntry();
      if (item.isFile) {
        const file = await new Promise((resolve) => item.file(resolve));
        if (isValidFile(file)) {
          filesArray.push(file);
        } else {
          setError(`Invalid file type. Only ${allowedExtensions.join(", ")} files are allowed.`);
        }
      } else if (item.isDirectory) {
        await traverseFileTree(item, filesArray);
      }
    }
    return filesArray;
  };

  const traverseFileTree = (item, filesArray) => {
    return new Promise((resolve) => {
      if (item.isFile) {
        item.file((file) => {
          if (isValidFile(file)) {
            filesArray.push(file);
          }
          resolve();
        });
      } else if (item.isDirectory) {
        const dirReader = item.createReader();
        dirReader.readEntries(async (entries) => {
          for (const entry of entries) {
            await traverseFileTree(entry, filesArray);
          }
          resolve();
        });
      }
    });
  };

  const isValidFile = (file) => allowedExtensions.some((ext) => file.name.endsWith(ext));

  const handleFileSelect = (e) => {
    const files = Array.from(e.target.files);
    validateAndSetFiles(files);
  };

  const validateAndSetFiles = (files) => {
    const filteredFiles = files.filter(isValidFile);
    if (filteredFiles.length + selectedFiles.length > maxFiles) {
      setError(`You can only upload a maximum of ${maxFiles} files.`);
    } else {
      setSelectedFiles((prev) => [...prev, ...filteredFiles].slice(0, maxFiles));
      setError("");
    }
  };

  return (
    <div
      onDragOver={handleDragOver}
      onDrop={handleDrop}
      class="p-4 border-2 border-dashed border-gray-300 rounded-md text-center cursor-pointer mb-4 h-96 w-96 flex overflow-y-scroll items-center justify-center"
    >
      <input
        type="file"
        onChange={handleFileSelect}
        style={{ display: "none" }}
        accept={allowedExtensions.join(",")}
        id="fileUpload"
        multiple
      />
      <label for="fileUpload" class="cursor-pointer">
        {selectedFiles.length > 0 ? (
          <div class="flex flex-wrap gap-10">
            {selectedFiles.map((file) => (
              <div key={file.name}>
                <DocumentTextIcon class="mx-auto w-8" />
                <p>{file.name}</p>
              </div>
            ))}
          </div>
        ) : (
          <div>
            <ArrowUpTrayIcon class="mx-auto w-8" />
            <p>Drag & drop files or folders here, or click to select files</p>
          </div>
        )}
      </label>
    </div>
  );
};

export default FileDropZone;
---

<FileDropZone />
  ```
  