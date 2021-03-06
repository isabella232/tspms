
# API Documentation

## Functions

### injectLogger

see [logger.ts](../src/main/logger.ts#L43-L47).

```
injectLogger(info: Logger, warn: Logger, error: Logger): void
```
  
Let the user inject logger used by the service.

* info: information logger.
* warn: warning logger.
* error: error logger.

### injectPromiseLibrary

see [promise.ts](../src/main/promise.ts#L262-L264).

```
injectPromiseLibrary(promise: typeof Promise): void
```
  
Let the user inject Promise library used by the service, 
it must be an es6 spec comliant promise library

* promise: the Promise constructor of the injected library.

### init

see [index.ts](../src/main/index.ts#L119-L121).

```
init(config: ProjectManagerConfig): Promise<void>
```
  
Initializate the service.

* config: the main service configuration

### updateProjectConfigs

see [index.ts](../src/main/index.ts#L137-L139).

```
updateProjectConfigs(configs: { [projectId: string]: TypeScriptProjectConfig; }): Promise<void>
```
  
Update the configurations of the projects managed by this service.

* configs: A map project name to project config file.
if a project previously managed by this service is not present in the  map
the project will be disposed. 
If a new project is present in the map, the project will be initialized
Otherwise the project will be updated accordingly to the new configuration

### dispose

see [index.ts](../src/main/index.ts#L145-L147).

```
dispose(): void
```
  
Dispose the service.

### getDiagnosticsForFile

see [index.ts](../src/main/index.ts#L245-L267).

```
getDiagnosticsForFile(fileName: string, allErrors?: boolean): Promise<Diagnostics[]>
```
  
Retrieve a list of errors for a given file
return a promise resolving to a list of errors

* fileName: the absolute file name
* allErrors: by default errors are checked in 3 phases, options check, syntax check, 
semantic check, is allErrors is set to false, the service won't check the nex phase 
if there is error in the precedent one

### getCompletionAtPosition

see [index.ts](../src/main/index.ts#L300-L357).

```
getCompletionAtPosition(fileName: string, position: number, limit?: number, skip?: number): Promise<CompletionResult>
```
  
Retrieve completion proposal at a given point in a given file.
return a promise resolving to a list of completion proposals.

* fileName: the absolute file name.
* position: the position in the file where the completion is requested.
* limit: the max number of proposition this service shoudl return.
* skip: the number of proposition this service should skip.

### getQuickInfoAtPosition

see [index.ts](../src/main/index.ts#L381-L393).

```
getQuickInfoAtPosition(fileName: string, position: number): Promise<QuickInfo>
```
  
Retrieve information about type/documentation for the givent file name at the given position.

* fileName: the absolute file name.
* position: the position in the file where the informations are requested.

### getSignatureHelpItems

see [index.ts](../src/main/index.ts#L417-L429).

```
getSignatureHelpItems(fileName: string, position: number): Promise<SignatureHelpItems>
```
  
Retrieve signature information about a function being called.

* fileName: the absolute file name.
* position: the position in the file where the informations are requested.

### getRenameInfo

see [index.ts](../src/main/index.ts#L455-L474).

```
getRenameInfo(fileName: string, position: number): Promise<RenameInfo>
```
  
Retrieve rename informations about a symbol at a given position.
This method will look into all the projects, and returns the first positive renameInfo found.

* fileName: the absolute file name.
* position: the position in the file where the rename informations are requested.

### findRenameLocations

see [index.ts](../src/main/index.ts#L489-L511).

```
findRenameLocations(fileName: string, position: number, findInStrings: boolean, findInComments: boolean): Promise<{ textSpan: TextSpan; fileName: string;}[]>
```
  
Retrieve locations where a rename must occurs. 
This methods apply to all the project that manage the given file.

* fileName: the absolute file name.
* position: the position of the symbol to rename.
* findInStrings
* findInComments: if true the service will also look into comments.

### getDefinitionAtPosition

see [index.ts](../src/main/index.ts#L535-L548).

```
getDefinitionAtPosition(fileName: string, position: number): Promise<DefinitionInfo[]>
```
  
Retrieve informations about a typescript definition.

* fileName: the absolute file name.
* position: the position of the definition in the file.

### getReferencesAtPosition

see [index.ts](../src/main/index.ts#L571-L590).

```
getReferencesAtPosition(fileName: string, position: number): Promise<ReferenceEntry[]>
```
  
Retrieve a symbol references accros a project.
This method look into every project that manage the given file.

* fileName: the absolute file name.
* position: the position of the symbol.

### getOccurrencesAtPosition

see [index.ts](../src/main/index.ts#L604-L614).

```
getOccurrencesAtPosition(fileName: string, position: number): Promise<ReferenceEntry[]>
```
  
Retrieve a symbol references accros a file.

* fileName: the absolute file name.
* position: the position of the symbol.

### getNavigateToItems

see [index.ts](../src/main/index.ts#L641-L665).

```
getNavigateToItems(search: string): Promise<NavigateToItem[]>
```
  
Retrieve information about navigation between files of the project

* search

### getNavigationBarItems

see [index.ts](../src/main/index.ts#L707-L712).

```
getNavigationBarItems(fileName: string): Promise<NavigationBarItem[]>
```
  
Retrieve navigation bar for the givent file

* fileName: the absolute file name.

### getFormattingEditsForFile

see [index.ts](../src/main/index.ts#L743-L761).

```
getFormattingEditsForFile(fileName: string, options: ts.FormatCodeOptions, start?: number, end?: number): Promise<TextChange[]>
```
  
Retrieve formating information for a file or range in a file.

* fileName: the absolute file name.
* options: formatting options.
* start: if start and end are provided the formatting will only be applied on that range.
* end: if start and end are provided the formatting will only be applied on that range.

### getFormattingEditsAfterKeyStroke

see [index.ts](../src/main/index.ts#L777-L791).

```
getFormattingEditsAfterKeyStroke(fileName: string, options: ts.FormatCodeOptions, position: number, key: string): Promise<TextChange[]>
```
  
Retrieve formating information after a key stroke (use for auto formating)

* fileName: the absolute file name.
* options: formatting options.
* position: the position where the key stroke occured.
* key: the key.

### getEmitOutput

see [index.ts](../src/main/index.ts#L803-L808).

```
getEmitOutput(fileName: string): Promise<ts.EmitOutput>
```
  
Retrieve emit output for a file name

* fileName: the absolute file name.


## Types

### ProjectManagerConfig

see [projectManager.ts](../src/main/projectManager.ts#L36-L56).
  


```
export type ProjectManagerConfig  = {
    /**
     * Absolute fileName of the `lib.d.ts` file associated to the bundled compiler.
     */
    defaultLibFileName: string;
    
    /**
     * The file system wrapper instance used by this module.
     */
    fileSystem: fs.IFileSystem;
    
    /**
     * Working set service.
     */
    workingSet: ws.IWorkingSet;
    
    /**
     * A Map project name to project configuration
     */
    projectConfigs: { [projectId: string]: TypeScriptProjectConfig; };
}
```

### IFileSystem

see [fileSystem.ts](../src/main/fileSystem.ts#L17-L40).
  
Interface abstracting file system to provide adapter to the service.

```
export interface IFileSystem {
    
    /**
     * Return a promise resolving to the current directory opened in the editor.
     */
    getCurrentDir(): promise.Promise<string>;
    
    /**
     * A signal dispatching change in files under the current directory.
     */
    projectFilesChanged: ISignal<FileChangeRecord[]>;
    
    /**
     * Return a promise that resolve to an array of string containing all the typescript files name in the projects.
     */
    getProjectFiles(): promise.Promise<string[]>;
    
    /**
     * Read a file, return a promise that resolve to the file content.
     * 
     * @param fileName the name of file to read.
     */
    readFile(fileName: string): promise.Promise<string>;
}
```

### FileChangeRecord

see [fileSystem.ts](../src/main/fileSystem.ts#L76-L86).
  


```
export type FileChangeRecord = {
    /**
     * kind of change.
     */
    kind: FileChangeKind;
    
    /**
     * The name of the file that have changed if any.
     */
    fileName?: string;
}
```

### FileChangeKind

see [fileSystem.ts](../src/main/fileSystem.ts#L51-L71).
  
An Enum representing the kind of change that migth occur in the fileSysem.

```
export const enum FileChangeKind {
    /**
     * A file has been added.
     */
    ADD,
    
    /**
     * A file has been updated.
     */
    UPDATE,
    
    /**
     * A file has been deleted.
     */
    DELETE,
    
    /**
     * The project files has been refreshed.
     */
    RESET
}
```

### IWorkingSet

see [workingSet.ts](../src/main/workingSet.ts#L17-L32).
  
A service that will reflect files in the working set of the editor.

```
export interface IWorkingSet {
    /**
     * The list of files open in the working set.
     */
    getFiles(): promise.Promise<string[]>;
    
    /**
     * A signal dispatching events when change occured in the working set.
     */
    workingSetChanged: ISignal<WorkingSetChangeRecord>;
    
    /**
     * A signal that provide fine grained change descriptor over edited documents.
     */
    documentEdited: ISignal<DocumentChangeRecord>;
}
```

### DocumentChangeDescriptor

see [workingSet.ts](../src/main/workingSet.ts#L79-L101).
  


```
export type DocumentChangeDescriptor = {
    
    /**
     * Start position of the change.
     */
    from?: number;
    
    /**
     * End positon of the change.
     */
    to?: number;
    
    /**
     * The text that has been inserted (if any).
     */
    text?: string;
    
    /**
     * The text that has been removed (if any).
     */
    removed?: string;

}
```

### DocumentChangeRecord

see [workingSet.ts](../src/main/workingSet.ts#L109-L124).
  


```
export type DocumentChangeRecord = {
    /**
     * absolute file name of the files that has changed.
     */
    fileName: string;
    
    /**
     * The list of changes that occured in the file.
     */
    changeList?: DocumentChangeDescriptor[];
    
    /**
     * The new text of the file.
     */
    documentText?: string
}
```

### WorkingSetChangeRecord

see [workingSet.ts](../src/main/workingSet.ts#L43-L53).
  


```
export type WorkingSetChangeRecord = {
    /**
     * The kind of change that occured in the working set.
     */
    kind: WorkingSetChangeKind;
    
    /**
     * The list of paths that has been added or removed from the working set.
     */
    fileNames: string[];
}
```

### WorkingSetChangeKind

see [workingSet.ts](../src/main/workingSet.ts#L58-L68).
  
An Enum listing the kind of change that might occur in the working set.

```
export const enum WorkingSetChangeKind {
    /**
     * A file has been added to the working set.
     */
    ADD,
    
    /**
     * A file has been removed from the working set.
     */
    REMOVE
}
```

### TypeScriptProjectConfig

see [project.ts](../src/main/project.ts#L26-L41).
  


```
export type TypeScriptProjectConfig = {
    /**
     * Patterns used for glob matching sources file of the project
     */
    sources: string[] | string;
    
    /**
     * Compiler options
     */
    compilerOptions: ts.CompilerOptions;
        
    /**
     * Absolute path of the compiler directory
     */
    compilerDirectory?: string;
}
```

### ISignal

see [utils.ts](../src/main/utils.ts#L137-L169).
  
C# like events and delegates for typed events dispatching.

```
export interface ISignal<T> {
    /**
     * Subscribes a listener for the signal.
     * 
     * @params listener the callback to call when events are dispatched.
     * @params priority an optional priority for this listerner
     */
    add(listener: (parameter: T) => any, priority?: number): void;
    
    /**
     * unsubscribe a listener for the signal
     * 
     * @params listener the previously subscribed listener
     */
    remove(listener: (parameter: T) => any): void;
    
    /**
     * Dispatch an event.
     * 
     * @params parameter the parameter attached to the event dispatched.
     */
    dispatch(parameter?: T): boolean;
    
    /**
     * Remove all listener from the signal.
     */
    clear(): void;
    
    /**
     * Returns true if listener has been subsribed to this signal.
     */
    hasListeners(): boolean;
}
```

### TextSpan

see [index.ts](../src/main/index.ts#L163-L173).
  


```
export type TextSpan = {
    /**
     * The start of the text span.
     */
    start: number;
    
    /**
     * The length of the text span.
     */
    length: number;
}
```

### Diagnostics

see [index.ts](../src/main/index.ts#L204-L234).
  


```
export type Diagnostics = {
    /**
     * The name of the file related to this diagnostic.
     */
    fileName: string;
    
    /**
     * Start position of the error.
     */
    start: number;
    
    /**
     * Length of the error.
     */
    length: number;
    
    /**
     * Error message.
     */
    messageText: string;
    
    /**
     * Diagnostic category. (warning, error, message)
     */
    category: ts.DiagnosticCategory;
    
    /**
     * Error code
     */
    code: number;
}
```

### CompletionResult

see [index.ts](../src/main/index.ts#L277-L287).
  


```
export type CompletionResult = {
    /**
     * the matched string portion
     */
    match: string;
    
    /**
     * list of proposed entries for code completion
     */
    entries: ts.CompletionEntryDetails[];
}
```

### QuickInfo

see [index.ts](../src/main/index.ts#L367-L373).
  


```
export type QuickInfo = {
    kind: string;
    kindModifiers: string;
    textSpan: TextSpan;
    displayParts: ts.SymbolDisplayPart[];
    documentation: ts.SymbolDisplayPart[];
}
```

### SignatureHelpItems

see [index.ts](../src/main/index.ts#L403-L409).
  


```
export type SignatureHelpItems = {
    items: ts.SignatureHelpItem[];
    applicableSpan: TextSpan;
    selectedItemIndex: number;
    argumentIndex: number;
    argumentCount: number;
}
```

### RenameInfo

see [index.ts](../src/main/index.ts#L438-L446).
  


```
export type RenameInfo = {
    canRename: boolean;
    localizedErrorMessage: string;
    displayName: string;
    fullDisplayName: string;
    kind: string;
    kindModifiers: string;
    triggerSpan: TextSpan;
}
```

### DefinitionInfo

see [index.ts](../src/main/index.ts#L520-L527).
  


```
export type DefinitionInfo = {
    fileName: string;
    textSpan: TextSpan;
    kind: string;
    name: string;
    containerKind: string;
    containerName: string;
}
```

### ReferenceEntry

see [index.ts](../src/main/index.ts#L557-L561).
  


```
export type ReferenceEntry = {
    textSpan: TextSpan;
    fileName: string;
    isWriteAccess: boolean;
}
```

### NavigateToItem

see [index.ts](../src/main/index.ts#L624-L633).
  


```
export type NavigateToItem = {
    name: string;
    kind: string;
    kindModifiers: string;
    matchKind: string;
    fileName: string;
    textSpan: TextSpan;
    containerName: string;
    containerKind: string;
}
```

### NavigationBarItem

see [index.ts](../src/main/index.ts#L675-L684).
  


```
export type NavigationBarItem = {
    text: string;
    kind: string;
    kindModifiers: string;
    spans: { start: number; length: number }[];
    childItems: NavigationBarItem[];
    indent: number;
    bolded: boolean;
    grayed: boolean;
}
```

### TextChange

see [index.ts](../src/main/index.ts#L723-L733).
  


```
export type TextChange = {
    /**
     * The text span to replace.
     */
    span: TextSpan;
    
    /**
     * The new text to insert.
     */
    newText: string;
}
```


