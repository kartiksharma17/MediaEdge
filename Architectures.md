## 🔐 Authentication Module

```mermaid
flowchart TD

A[Client Request] --> B[Auth Controller]
B --> C[Authentication Service]
C --> D{Request Type}

D -->|Register| E[Validate User]
E --> F[BCrypt Password Hashing]
F --> G[Save User]
G --> H[(MySQL)]

D -->|Login| I[Verify Credentials]
I --> J[BCrypt Password Match]
J --> H
H --> K[Generate JWT]
K --> L[Return Token]
```

## 🎬 Anime/Media Module

```mermaid
flowchart TD

A[Anime Request]
A --> B[Anime Controller]
B --> C[Anime Service]
C --> D[Anime Repository]
D --> E[(MySQL)]

E --> F[Anime Metadata]
F --> G[Genres]
F --> H[Poster URLs]
F --> I[Synopsis]
F --> J[Ratings]

J --> K[JSON Response]
```

## 📺 Episode Module

```mermaid
flowchart TD

A[Episode Request]
A --> B[Episode Controller]
B --> C[Episode Service]
C --> D[Episode Repository]
D --> E[(MySQL)]

E --> F[Episode Details]
F --> G[Duration]
F --> H[Thumbnail]
F --> I[HLS Playlist]
F --> J[Available Qualities]
```

## 🔍 Search Module

```mermaid
flowchart TD

A[Search Query]
A --> B[Search Controller]
B --> C[Search Service]
C --> D{Redis Cache?}

D -->|Hit| E[Return Results]

D -->|Miss| F[(MySQL)]
F --> G[Store in Redis]
G --> E
```

## 📜 Watch History Module

```mermaid
flowchart TD

A[Video Playback]
A --> B[Update Position]
B --> C[History Service]
C --> D[(MySQL)]

D --> E[Episode]
D --> F[Timestamp]
D --> G[Continue Watching]
```

## ▶️ Streaming Module

```mermaid
flowchart TD

A[Player]
A --> B[Streaming Controller]
B --> C[Streaming Service]
C --> D[MediaEdge CDN]
D --> E[Chunk Delivery]

E --> F[master.m3u8]
E --> G[segment001.ts]
E --> H[segment002.ts]
```

## 🚀 MediaEdge CDN Module

```mermaid
flowchart TD

A[Chunk Request]
A --> B[Cache Lookup]

B --> C{Chunk Exists?}

C -->|Yes| D[Return Chunk]

C -->|No| E[Origin Client]

E --> F[Download Chunk]

F --> G[Store in SSD Cache]

G --> D

G --> H[Update Cache Metrics]
```

## 💾 Cache Module

```mermaid
flowchart TD

A[Chunk Stored]
A --> B[Cache Manager]

B --> C[LRU Eviction]

B --> D[TTL Expiration]

B --> E[Disk Cleanup]

B --> F[Cache Metrics]
```

## 📦 Origin Module

```mermaid
flowchart TD

A[Origin Client]

A --> B[MinIO]

B --> C[master.m3u8]

B --> D[720p]

B --> E[1080p]

D --> F[segment001.ts]

D --> G[segment002.ts]

E --> H[segment003.ts]
```

## ⬇️ Download Module

```mermaid
flowchart TD

A[Download Request]

A --> B[JWT Validation]

B --> C[Permission Check]

C --> D{Authorized?}

D -->|No| E[Access Denied]

D -->|Yes| F[Generate Temporary Token]

F --> G[Download MP4]
```

## 📈 Analytics Module

```mermaid
flowchart TD

A[Streaming Events]

A --> B[Analytics Service]

B --> C[Views]

B --> D[Downloads]

B --> E[Cache Hits]

B --> F[Cache Misses]

B --> G[Streaming Time]

B --> H[Popular Anime]
```

## ⚡ Redis Architecture

```mermaid
flowchart TD

A[Redis]

A --> B[Search Cache]

A --> C[Continue Watching]

A --> D[JWT Blacklist]

A --> E[Cache Metadata]

A --> F[Popular Anime Cache]
```

## 🗄️ MySQL Architecture

```mermaid
flowchart TD

A[(MySQL)]

A --> B[Users]

A --> C[Anime]

A --> D[Episodes]

A --> E[History]

A --> F[Downloads]
```


## 🎥 FFmpeg Processing

```mermaid
flowchart TD

A[Original MP4]

A --> B[FFmpeg]

B --> C[master.m3u8]

B --> D[720p]

B --> E[1080p]

D --> F[Video Segments]

E --> G[Video Segments]

F --> H[MinIO]

G --> H
```

## ✅ Cache Hit Flow

```mermaid
flowchart TD

A[Player]

A --> B[Chunk Request]

B --> C[MediaEdge CDN]

C --> D[Local SSD Cache]

D --> E[Chunk Found]

E --> F[Return Chunk]
```

## ❌ Cache Miss Flow

```mermaid
flowchart TD

A[Player]

A --> B[Chunk Request]

B --> C[MediaEdge CDN]

C --> D[Cache Miss]

D --> E[Origin Client]

E --> F[MinIO]

F --> G[Download Chunk]

G --> H[Store in SSD Cache]

H --> I[Return Chunk]
```

## ☁️ Future Expansion

```mermaid
flowchart TD

A[MediaEdge CDN]

A --> B[Origin Client]

B --> C[MinIO]

B --> D[AWS S3]

B --> E[Cloudflare R2]

B --> F[Azure Blob Storage]
```

## 🏛️ Final Component Relationship

```mermaid
flowchart TD

A[React Frontend]

A --> B[Controllers]

B --> C[Services]

C --> D[(MySQL)]

C --> E[(Redis)]

C --> F[MediaEdge CDN]

F --> G[Local SSD Cache]

F --> H[Origin Client]

H --> I[MinIO]
```
