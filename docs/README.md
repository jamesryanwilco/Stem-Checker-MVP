# Stem Checker MVP

## Overview

Stem Checker is a lightweight macOS utility designed for audio professionals, including music producers, mixing engineers, and mastering engineers. It provides a quick and seamless way to check bounced audio stems for synchronization issues directly from Finder, without the need to open a full-featured Digital Audio Workstation (DAW).

The core goal is to accelerate the quality assurance workflow, saving valuable time when verifying exports before they are sent to clients, collaborators, or mastering houses.

## Core Features

- **Synchronized Playback:** Select multiple audio files (`.wav`, `.aiff`) in Finder and play them back simultaneously.
- **Finder Integration:** The app is accessible via a right-click "Check Stems" Quick Action, making it a natural part of the file management workflow.
- **Minimalist UI:** A simple, non-intrusive player window with basic transport controls (Play/Stop).
- **Efficiency:** Drastically reduces the time it takes to perform routine stem checks by avoiding the overhead of launching a DAW.

## How It Works

1.  **Select Stems:** The user selects two or more audio files in a Finder window.
2.  **Right-Click:** The user right-clicks the selection and chooses "Check Stems" from the Quick Actions menu.
3.  **Launch & Load:** The Stem Checker application launches, automatically loading the selected files.
4.  **Playback:** The user can press "Play" to hear all stems playing in perfect sync.

This repository contains the source code for the MVP version of the application, built natively for macOS using Swift and SwiftUI.
