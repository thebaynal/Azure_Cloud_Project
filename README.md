# Azure Deployment Documentation

## Overview
This document provides step-by-step instructions for deploying the MaScan QR Attendance Checker application on Microsoft Azure.

## Project Overview, Team Members, Video Link, and Demo URL

**Project**: MaScan QR Attendance Checker - Azure Cloud Deployment

**Team Members:**
- John Raymon Alba
- Fredrick Ortinero
- Divino Al Ricafort
- Mark Vincent Raña

**Video Link**: https://youtu.be/sHimFjR4bxU

**Demo URL**: https://mascan-qr-acexb0a8febaacab.southeastasia-01.azurewebsites.net/login

## Team Members
- John Raymon Alba
- Fredrick Ortinero
- Divino Al Ricafort
- Mark Vincent Raña

## Prerequisites
- Azure for Students account (or paid subscription)
- Flask application code
- Azure Portal access
- Local Python 3.10+ environment

## Deployment Architecture

The application is deployed across the following Azure services:

| Resource | Type | Location | Purpose |
|----------|------|----------|---------|
| mascan-rg | Resource Group | Southeast Asia | Container for all resources |
| mascan-qr | App Service | Southeast Asia | Host Flask web application |
| ASP-mascarqr | App Service Plan | Southeast Asia | Compute resources for App Service |
| mascan-sql | Azure SQL Database | Southeast Asia | Production database |
| mascanstorage | Storage Account | Southeast Asia | Blob storage for uploads |


