# Architecture Diagram

PhotoAlbum is an ASP.NET Core 9.0 Razor Pages web application for photo gallery management, using Entity Framework Core with SQL Server for metadata and local file storage for image files.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nHTTP Client"]

    subgraph App["ASP.NET Core 9.0 Web Application"]
        subgraph Presentation["Presentation Layer - Razor Pages"]
            Index["Index.cshtml\nGallery Grid + Upload"]
            Detail["Detail.cshtml\nFull-size Photo + Navigation"]
            PhotoFile["PhotoFile.cshtml\nFile Retrieval Endpoint"]
        end

        subgraph Service["Service Layer"]
            IPhotoService["IPhotoService"]
            PhotoService["PhotoService\nUpload Validation\nImage Processing\nFile Management"]
            ImageSharp["SixLabors.ImageSharp\nDimension Extraction"]
        end

        subgraph Data["Data Access Layer - EF Core 9.0"]
            DbContext["PhotoAlbumContext\nDbContext"]
            PhotoModel["Photo Model\nMetadata Entity"]
        end
    end

    subgraph Storage["Storage"]
        SQLServer["SQL Server LocalDB\nPhotoAlbumDb\nPhoto Metadata"]
        FileSystem["Local File System\nwwwroot/uploads\nGUID-named Image Files"]
    end

    Browser -->|"HTTP requests"| Presentation
    Index -->|"upload / list photos"| IPhotoService
    Detail -->|"get photo"| IPhotoService
    PhotoFile -->|"retrieve file"| IPhotoService
    IPhotoService --> PhotoService
    PhotoService --> ImageSharp
    PhotoService --> DbContext
    DbContext --> PhotoModel
    DbContext -->|"EF Core SQL Server"| SQLServer
    PhotoService -->|"read / write image files"| FileSystem
    PhotoFile -->|"serve static image"| FileSystem
```
