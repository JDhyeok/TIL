# 이슈

## 이슈 재현

### 재현 중에 생긴 이슈사항

Azure Portal에서 Empty folder를 재현하기 위해서 folder-1/file, folder-12/file을 만들고 한 쪽 directory 안에 file을 삭제했다. 그와 동시에 해당 empty directory가 사라져서 이슈를 재현하는데 문제가 생겼다.

### 해결 방법

서치 결과 Azure Blob Strage "폴더"라는 개념은 존재하지 않는다고 한다. 우리가 폴더라고 부르는 것은 그냥 blob 이름의 prefix이다. folder-1/file의 folder-1은 그냥 prefix인 것이다. 따라서 folder-1/file을 삭제하게 되면 folder-1도 바로 삭제가 된다.

따라서 그냥 Blob 형식의 **folder-1**과 **folder-12/file**을 만들어 놓고 에러 코드를 구현해 봤다.

## 재현 코드 및 에러

- Bug 재현 코드
```java
    Map<String, Object> config = new HashMap<>();
    String stores = "<container_name>,<another_container_name>"; // A comma separated list of container names
    StorageSharedKeyCredential credential = new StorageSharedKeyCredential("<account_name", "account_key");
    config.put(AzureFileSystem.AZURE_STORAGE_SHARED_KEY_CREDENTIAL, credential);
    config.put(AzureFileSystem.AZURE_STORAGE_FILE_STORES, stores);
    FileSystem myFs = FileSystems.newFileSystem(new URI("azb://?endpoint=<account_endpoint"), config);

    Path dirPath1 = myFs.getPath("notemptytest:/folder-1");
    Files.delete(dirPath1);
```

- 에러 메시지
  
  Issue #24193에서 말했듯이 빈 디렉토리(Blob)을 삭제함에도 불구하고 DirectoryNotEmptyException이 나온다.


```
java.nio.file.DirectoryNotEmptyException: notemptytest:/folder-1
	at com.azure.storage.blob.nio.AzureFileSystemProvider.delete(AzureFileSystemProvider.java:622)
	at java.base/java.nio.file.Files.delete(Files.java:1146)
	at com.example.demo.Delete.main(Delete.java:36)
```

### 이슈 url
https://github.com/Azure/azure-sdk-for-java/issues/24193

## Ref

https://supportcenter.devexpress.com/ticket/details/t1006160/file-manager-how-to-correctly-create-empty-folders-in-azure-blob-storage-with-the-latest

https://docs.microsoft.com/en-us/answers/questions/466968/remove-azure-blob-storage-folders-with-sdk.html