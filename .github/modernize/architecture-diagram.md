# Architecture Diagram

PhotoAlbum is an ASP.NET Core 9.0 Razor Pages web application for photo gallery management, using Entity Framework Core with SQL Server and local file system storage.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nUser Interface"]

    subgraph WebApp["ASP.NET Core 9.0 Web Application"]
        subgraph Pages["Presentation Layer - Razor Pages"]
            Index["Index.cshtml\nGallery Grid + Upload"]
            Detail["Detail.cshtml\nPhoto Detail + Navigation"]
            PhotoFile["PhotoFile.cshtml\nFile Retrieval Endpoint"]
        end

        subgraph Services["Service Layer"]
            IPhotoService["IPhotoService\nInterface"]
            PhotoService["PhotoService\nUpload, Validate, Retrieve, Delete"]
            ImageSharp["SixLabors.ImageSharp 3.x\nImage Dimension Extraction"]
        end

        subgraph Data["Data Access Layer"]
            DbContext["PhotoAlbumContext\nEF Core 9.0 DbContext"]
            PhotoModel["Photo Model\nFilename, Size, MimeType, Dimensions, Timestamps"]
        end
    end

    subgraph Storage["Storage"]
        SQLServer["SQL Server LocalDB\nPhotoAlbumDb\nPhoto Metadata"]
        FileSystem["Local File System\nwwwroot/uploads\nGUID-based Image Files"]
    end

    Browser -- "HTTP Requests" --> Pages
    Pages -- "Calls" --> IPhotoService
    IPhotoService -- "Implemented by" --> PhotoService
    PhotoService -- "Processes images" --> ImageSharp
    PhotoService -- "Persists metadata" --> DbContext
    PhotoService -- "Stores files" --> FileSystem
    DbContext -- "SQL queries" --> SQLServer
```
