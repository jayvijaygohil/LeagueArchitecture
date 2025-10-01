# 🛡️ Android Architecture: Claims and Document Management App

This document outlines the high-level architecture and implementation plan for an Android application focused on managing claims and documents, designed around a modern, layered approach using **Jetpack Compose** and **MVI (Model-View-Intent)** principles.

---

## 🏗️ High-Level Plan

The application is structured into a standard Android architectural pattern, consisting of three main layers: Data, Domain, and Presentation. Background work will be managed via **WorkManager** to handle synchronization tasks.

---

## 💾 Data Layer

The Data Layer is responsible for abstracting data sources and handling data persistence and remote access.

| Component | Description |
| :--- | :--- |
| **Local Data Source** | Uses **Room Database** to store locally and manage data for claims and documents that are pending synchronization with the backend. |
| **Remote Data Source** | An **API Client** responsible for all interactions with the backend API. |
| **Repository** | Abstracts data access logic. It decides whether to fetch data from the local or remote source and handles the synchronization logic. |
| **Background Work** | Utilizes **WorkManager** for reliably uploading pending claims and documents in the background. |

---

## ⚙️ Domain Layer

The Domain Layer contains the core business logic of the application, independent of any specific UI or data source implementation.

| Component | Description |
| :--- | :--- |
| **Use Cases/Interactors** | Define the specific business operations, such as: `Upload Claim/Document`, `Get Claims`, `Get Documents`, `Edit Claim/Document`, etc. |
| **Entities** | Defines the core data structures used throughout the application, specifically the **Claim** and **Document** entities. |

---

## 🎨 Presentation Layer (MVI)

The Presentation Layer is responsible for handling UI logic, state management, and user interaction, following the **Model-View-Intent (MVI)** architectural pattern.

| Component | Description |
| :--- | :--- |
| **Presenters** | Implemented using **Compose functions**, these handle the UI logic and manage the state of the associated UI screen. |
| **MVI Intents/Actions** | User actions (e.g., button clicks, text input) that trigger state changes within the application. |
| **MVI States** | Represents the distinct states of the UI for a given view (e.g., `Loading`, `Success`, `Error`, `Data Ready`). |

---

## 📱 UI Layer (Jetpack Compose)

The UI Layer is built entirely with **Jetpack Compose** for a modern, declarative, and state-driven user interface.

| Screen Type | Description |
| :--- | :--- |
| **Composable Functions** | Defines the screens for **Lists**, **Details**, and **Add** functionality for both Claims and Documents. |
| **Wiring** | Connects the **Presenters** with the respective **UI Screens** to manage the flow of data and state. |

---

## ✅ Initial Implementation Assumptions

* A pre-existing **Backend API** is available for claims and documents management.
* The initial scope allows for **single image upload** per Claim or Document.
* **Image selection** will be implemented using the device's default gallery/image picker.
* The diagram for error handling and network checks will be simplified, but the implementation is expected to use a robust **`LeagueException`**-based system.

---

## 🚀 Future Improvements and Enhancements

These items are planned for subsequent iterations:

### Core Functionality
* **Upload Progress Indicators:** Displaying upload status on Home and List screens.
* **Upload Cancellation:** Ability to stop an in-progress background upload.
* **Claim/Document Deletion:** Implementing logic for removing saved items.
* **Document Download:** Adding functionality to download documents from the backend.

### Media & File Management
* **Multiple Images:** Support for uploading more than one image per Claim/Document.
* **File Type Support:** Extending support to various file types beyond images.
* **In-App Capture:** Integration of **CameraX** or a third-party document scanning SDK for in-app image capture.

### User Experience & Intelligence
* **Search and Filtering:** Implementing robust search and filtering capabilities based on criteria like **name, date, status, amount, and type**.
* **Intelligent Auto-fill:** Utilizing **Google ML Kit** for text recognition (OCR) to automatically extract metadata and populate form fields.
