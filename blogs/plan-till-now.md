I want to build Semantic Search Web app which fetches content from github api and makes it semantic friendly.

Next Js + Next Auth + Shadcn + Supabase + pgboss

Three Jobs in System

1. Process Repository Job
2. Chunk Generation Job
3. Embedding Generation Job

This is Schema

model User {
id String @id @default(cuid())
name String?
email String? @unique
emailVerified DateTime?
image String?
role UserRole @default(USER)
repositories Repository[]
}

model Repository {
id String @id @default(cuid())
githubId Int @unique
name String
fullName String
description String?
status RepositoryStatus @default(PENDING)
owner String
url String
userId String
user User @relation(fields: [userId], references: [id])
chunks RepositoryChunk[]
}

model RepositoryChunk {
id String @id @default(cuid())
repositoryId String
repository Repository @relation(fields: [repositoryId], references: [id])
content String
type String
filepath String
embeddings Float[]
keywords String[]
}

enum UserRole {
GUEST,  
 USER,  
 ADMIN  
}

enum RepositoryStatus {
PENDING
SUCCESS
}

Processing Pipeline flow :

1. Repo Submission
   - User enter github repo url
   - Frontend validates url -
   - if status is PENDing then start listening
   - if status is Success redirect to /repo/id
   - if repo does not exist then start processing

- There might be users who login with google for them i cannot give option of import github repo
- Users who login with github should have both options either enter github repo url or import github repo

Can you help me plan the User Flow too ?
You can make necessary changes to ui routes

1. Public Routes
   / # Landing/Home Page
   /search # Repository Search Interface
   /repository/[id] # Individual Repository Details
   /explore # Knowledge Graph Visualization
   /auth/login # GitHub Authentication / Google Authentication

2. Private Routes
   /dashboard # User's Saved Repositories
   /profile # User Profile and Settings
   /repository/new, # New Repository Submission
   /repository/import # GitHub Repository Import
