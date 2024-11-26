# GitHub API Testing for Repositories, Commits, and Contents

## Overview
This project demonstrates how to use the GitHub API to perform common tasks related to repositories, commits, and repository contents. The JSON collection provided outlines requests that can be tested with tools like Postman or custom Python scripts.

### Features Covered:
1. **Repository Content Operations**
   - Retrieve repository content.
   - Download repository archive (ZIP format).
   - List repositories for authenticated and specific users.

2. **Repository Search**
   - Search repositories using keywords, qualifiers, and sort options.

3. **Commit Operations**
   - List commits in a repository.
   - Retrieve details of a specific commit.

---

## Prerequisites

1. **GitHub Token**: 
   - Generate a [personal access token](https://github.com/settings/tokens) with the required scopes (Content and Commit read and write).
   - Replace `{{GITHUB_TOKEN}}` in the provided JSON `GitHub.postman_environment.json`.

2. **Postman Setup**:
   - Import the `GitHub.postman_environment.json` environment JSON file into Postman environments.
   - Set `GitHub` as active Postman environment
   - Import the `Github API Test.postman_collection.json` collection JSON file into Postman to test API calls directly.
   - Authorization for whole collection is set to Bearer Token with `{{GITHUB_TOKEN}}` as a Token.   
   If not, set it up as shown on a picture below. For folders inside collection select `Inherit auto from parent` as Auth Type  
   ![изображение](https://github.com/user-attachments/assets/78a1b389-13c7-4e8a-94a9-becc3c40c7e6)

3. **Environment Variables**:
   - Set up the following variables:
     - `{{API_BASE}}`: Is already set to `https://api.github.com`, no need for change.
     - `{{owner}}`: GitHub repository owner. For requests to the `{{GITHUB_TOKEN}}` owner's private repo and/or data.
     - `{{repo}}`: Repository name.
     - `{{REF}}`: Specific reference (branch/tag/commit hash).
     - `{{user}}`: GitHub username. For requests to other GitHub user's repo and/or data

---

## Authorization

This project uses **Bearer Token** for authentication:
- `Authorization: Bearer {{GITHUB_TOKEN}}` Must be included in the headers of every request to private data.
- If Postman set up is done properly, it will be included in request headers.
> **Treat your `{{GITHUB_TOKEN}}` as a secret!**

---

## API Version


Requests without the X-GitHub-Api-Version header will default to use the `2022-11-28` version.   
If you specify an API version that is no longer supported, you will receive a 400 error.  
The following REST API versions are currently supported: `2022-11-28`
>header "X-GitHub-Api-Version:2022-11-28"

---

## Requests Overview

### **1. Repository Content Operations**

#### a. Get Repository Content
- **Endpoint**: `GET /repos/{{owner}}/{{repo}}/contents`
- **Description**: Gets the contents of the repository's root directory.
- **Parameters**:
  - `ref`: Commit/branch/tag. Defaults to the repository’s default branch.
- [**Official Documentation**](https://docs.github.com/en/rest/repos/contents?apiVersion=2022-11-28#get-repository-content)

#### b. Download Repository Archive (ZIP)
- **Endpoint**: `GET /repos/{{owner}}/{{repo}}/zipball/{{REF}}`
- **Description**: Downloads the repository as a ZIP file.
- [**Official Documentation**](https://docs.github.com/en/rest/repos/contents?apiVersion=2022-11-28#download-a-repository-archive-zip)

#### c. List Repositories for Authenticated User
- **Endpoint**: `GET /user/repos`
- **Description**: Lists repositories that the authenticated user has explicit permission (:read, :write, or :admin) to access.
- **Parameters**:
  - `visibility`: Limit results by visibility (`all`, `public`, `private`).
  - `affiliation`: Filter repositories by affiliation (`owner`, `collaborator`, etc.).
- [**Official Documentation**](https://docs.github.com/en/rest/repos/repos?apiVersion=2022-11-28#list-repositories-for-the-authenticated-user)

#### d. List Repositories for a User
- **Endpoint**: `GET /users/{{user}}/repos`
- **Description**: Lists public repositories for the specified user.
- **Parameters**:
  - `type`: Repository type (`all`, `owner`, `member`).
  - `sort`: Sorting criteria (`created`, `updated`, etc.).
  - `direction`: Sort direction (`asc`, `desc`).
- [**Official Documentation**](https://docs.github.com/en/rest/repos/repos?apiVersion=2022-11-28#list-repositories-for-a-user)

---

### **2. Repository Search**

#### a. Search Repositories
- **Endpoint**: `GET /search/repositories`
- **Description**: Find repositories via various criteria. This method returns up to 100 results per page.
- **Parameters**:
  - `q`: Keywords and qualifiers (e.g., `pytorch+language:C`).
  - `sort`: Sorting criteria (`stars`, `forks`, etc.).
  - `order`: Sort order (`asc`, `desc`).
  - `per_page`: Results per page.
  - `page`: Page number.
- [**Official Documentation**](https://docs.github.com/en/rest/search/search?apiVersion=2022-11-28#search-repositories)
- [**More about search and search qualifiers**](https://docs.github.com/en/search-github/searching-on-github/searching-for-repositories)

---

### **3. Commit Operations**

#### a. List Commits
- **Endpoint**: `GET /repos/{{owner}}/{{repo}}/commits`
- **Description**: Returns the list of commits for user/repo. The response will include a verification object that describes the result of verifying the commit's signature. This endpoint can be used without authentication or the aforementioned permissions if only public resources are requested.
- **Parameters**:
  - `sha`: Commit SHA.
  - `author`: Commit author.
  - `since`, `until`: Filter commits by time range.
- [**Official Documentation**](https://docs.github.com/en/rest/commits/commits?apiVersion=2022-11-28#list-commits)

#### b. Get a Commit
- **Endpoint**: `GET /repos/{{owner}}/{{repo}}/commits/{{REF}}`
- **Description**: Returns the contents of a single commit reference. You must have read access for the repository to use this endpoint. The response will include a verification object that describes the result of verifying the commit's signature.
- **Parameters**:
  - `page`, `per_page`: Pagination options.
- [**Official Documentation**](https://docs.github.com/en/rest/commits/commits?apiVersion=2022-11-28#list-commits)

---

## Usage Instructions

### 1. Testing with Postman
1. [Set up Postman](#prerequisites)
2. Set up the environment variables if needed.
3. Run the requests to see results.

---

## Notes

- Ensure proper authentication to avoid [**rate limiting**](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api?apiVersion=2022-11-28).
- Replace `{{variable}}` placeholders with actual values.
- Use [**pagination**](https://docs.github.com/en/rest/using-the-rest-api/using-pagination-in-the-rest-api?apiVersion=2022-11-28) for large datasets.

---

## Resources

- [GitHub API Documentation](https://docs.github.com/en/rest)
- [Postman Download](https://www.postman.com/downloads/)
