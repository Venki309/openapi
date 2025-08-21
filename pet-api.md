openapi: 3.0.0
info:
  title: Pet Store API
  description: API for managing pets of different types
  version: 1.0.0
tags:
  - name: Pet Management
    description: Operations for managing pets (add, update, delete)
  - name: Pet Information
    description: Operations for retrieving pet details and searching for pets
  - name: Adoption
    description: Operations related to pet adoption
servers:
  - url: https://api.petstore.com/v1
paths:
  /pets:
    get:
      summary: Get all pets
      description: Retrieve a list of all pets in the store.
      tags:
        - Pet Information
      responses:
        '200':
          $ref: './responses/pet-list-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'
    post:
      summary: Add a new pet
      description: Create a new pet record in the system.
      tags:
        - Pet Management
      requestBody:
        content:
          application/json:
            schema:
              $ref: './schemas/pet.yaml'
      responses:
        '201':
          description: Pet created successfully
        '400':
          $ref: './responses/pet-error-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'
  /pets/{petId}:
    get:
      summary: Get pet details by ID
      description: Retrieve detailed information about a pet by its ID.
      tags:
        - Pet Information
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          $ref: './responses/pet-detail-response.yaml'
        '404':
          $ref: './responses/pet-error-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'
    put:
      summary: Update a pet's details
      description: Update the details of an existing pet by its ID.
      tags:
        - Pet Management
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
      requestBody:
        content:
          application/json:
            schema:
              $ref: './schemas/pet.yaml'
      responses:
        '200':
          description: Pet updated successfully
        '400':
          $ref: './responses/pet-error-response.yaml'
        '404':
          $ref: './responses/pet-error-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'
    delete:
      summary: Delete a pet
      description: Remove a pet record from the system by its ID.
      tags:
        - Pet Management
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
      responses:
        '204':
          description: Pet deleted successfully
        '404':
          $ref: './responses/pet-error-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'
  /pets/search:
    get:
      summary: Search pets by type
      description: Find pets based on their type (e.g., dog, cat).
      tags:
        - Pet Information
      parameters:
        - name: type
          in: query
          required: true
          schema:
            type: string
      responses:
        '200':
          $ref: './responses/pet-list-response.yaml'
        '400':
          $ref: './responses/pet-error-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'
  /pets/{petId}/adopt:
    post:
      summary: Mark a pet as adopted
      description: Mark a specific pet as adopted in the system.
      tags:
        - Adoption
      parameters:
        - name: petId
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Pet marked as adopted
        '404':
          $ref: './responses/pet-error-response.yaml'
        '500':
          $ref: './responses/pet-error-response.yaml'

components:
  schemas:
    Pet:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        type:
          type: string
          enum: [dog, cat, bird, fish, reptile]
        age:
          type: integer
        isAdopted:
          type: boolean
          description: Whether the pet has been adopted
      required:
        - name
        - type

responses:
  pet-error-response:
    description: Error response
    content:
      application/json:
        schema:
          type: object
          properties:
            errorCode:
              type: string
              example: PET_001
            message:
              type: string
              example: "An error occurred while processing your request."
            details:
              type: string
              example: "Invalid pet ID provided."
