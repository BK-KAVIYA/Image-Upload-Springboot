# Image Upload and Download Project

In this project, we will demonstrate how to upload an image into a database, download it back, and also upload an image to the file system while storing the file path in the database and fetching it back. We will be using Spring Boot, MySQL, and Postman for testing out APIs.

This project will be useful if you are implementing any real-world application where you need to store images and then send them back to the user when requested.

## API Endpoints

### Upload Image to Database

- **URL:** `/image`
- **HTTP Method:** POST
- **Request Parameter:** `image` (MultipartFile)
- **Description:** Uploads an image to the database.
- **Response:** Returns the path of the uploaded image.

### Download Image from Database

- **URL:** `/image/{fileName}`
- **HTTP Method:** GET
- **Path Variable:** `fileName` (String)
- **Description:** Downloads an image from the database by specifying the file name.
- **Response:** Returns the image data with the content type set to "image/png."

### Upload Image to File System

- **URL:** `/image/fileSystem`
- **HTTP Method:** POST
- **Request Parameter:** `image` (MultipartFile)
- **Description:** Uploads an image to the file system and stores the file path in the database.
- **Response:** Returns the path of the uploaded image.

### Download Image from File System

- **URL:** `/image/fileSystem/{fileName}`
- **HTTP Method:** GET
- **Path Variable:** `fileName` (String)
- **Description:** Downloads an image from the file system by specifying the file name.
- **Response:** Returns the image data with the content type set to "image/png."

## Controller Code

```java
@RestController
@RequestMapping("/image")
public class ImageUploadController {

    @Autowired
    private StorageService service;

    @PostMapping
    public ResponseEntity<?> uploadImage(@RequestParam("image") MultipartFile file) throws IOException {
        String uploadImage = service.uploadImage(file);
        return ResponseEntity.status(HttpStatus.OK)
                .body(uploadImage);
    }

    @GetMapping("/{fileName}")
    public ResponseEntity<?> downloadImage(@PathVariable String fileName){
        byte[] imageData=service.downloadImage(fileName);
        return ResponseEntity.status(HttpStatus.OK)
                .contentType(MediaType.valueOf("image/png"))
                .body(imageData);
    }

    @PostMapping("/fileSystem")
    public ResponseEntity<?> uploadImageToFIleSystem(@RequestParam("image")MultipartFile file) throws IOException {
        String uploadImage = service.uploadImageToFileSystem(file);
        return ResponseEntity.status(HttpStatus.OK)
                .body(uploadImage);
    }

    @GetMapping("/fileSystem/{fileName}")
    public ResponseEntity<?> downloadImageFromFileSystem(@PathVariable String fileName) throws IOException {
        byte[] imageData=service.downloadImageFromFileSystem(fileName);
        return ResponseEntity.status(HttpStatus.OK)
                .contentType(MediaType.valueOf("image/png"))
                .body(imageData);
    }
}
