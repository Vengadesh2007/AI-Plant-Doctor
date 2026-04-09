🧪 EXPERIMENT – 1: CRUD Operations
🎯 Aim

To perform Create, Read, Update, and Delete operations for plant disease records.

💻 Query
use aiPlantDoctorDB

// CREATE
db.plants.insertOne({
  plantId: "PLT101",
  plantName: "Tomato",
  disease: "Leaf Curl",
  confidence: 85,
  isHealthy: false
})

db.plants.insertMany([
  {
    plantId: "PLT102",
    plantName: "Potato",
    disease: "Late Blight",
    confidence: 92,
    isHealthy: false
  },
  {
    plantId: "PLT103",
    plantName: "Rice",
    disease: "Healthy",
    confidence: 98,
    isHealthy: true
  }
])

// READ
db.plants.find()
db.plants.find({ confidence: { $gt: 90 } })

// UPDATE
db.plants.updateOne(
  { plantId: "PLT101" },
  { $set: { confidence: 88 } }
)

db.plants.updateMany(
  { isHealthy: true },
  { $set: { status: "Healthy" } }
)

// DELETE
db.plants.deleteOne({ plantId: "PLT103" })
db.plants.deleteMany({ isHealthy: false })
📌 Output

Plant records inserted, retrieved, updated, and deleted successfully.

✅ Result

CRUD operations were successfully performed.

🧪 EXPERIMENT – 2: Collection Creation & Dropping
🎯 Aim

To create and drop a collection.

💻 Query
use aiPlantDoctorDB

db.createCollection("plants")

show collections

db.plants.drop()
📌 Output

Collection created, displayed, and dropped successfully.

✅ Result

The collection was successfully created and removed.

🧪 EXPERIMENT – 3: Required Fields & Data Types
🎯 Aim

To enforce required fields using schema validation.

💻 Query
db.createCollection("plants", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["plantId", "plantName", "confidence"],
      properties: {
        plantId: { bsonType: "string" },
        plantName: { bsonType: "string" },
        disease: { bsonType: "string" },
        confidence: { bsonType: "int" },
        isHealthy: { bsonType: "bool" }
      }
    }
  }
})
✔ Valid Insert
db.plants.insertOne({
  plantId: "PLT201",
  plantName: "Wheat",
  disease: "Rust",
  confidence: 80,
  isHealthy: false
})
❌ Invalid Insert
db.plants.insertOne({
  plantId: "PLT202",
  disease: "Blight"
})
📌 Output

Valid insert succeeds; invalid insert fails.

✅ Result

Schema validation enforced successfully.

🧪 EXPERIMENT – 4: Advanced Validation
🎯 Aim

To apply enum, range, and pattern validation.

💻 Query
db.createCollection("diagnosis", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["diagId", "plantName", "severity"],
      properties: {
        diagId: {
          bsonType: "string",
          pattern: "^DIA[0-9]{3}$"
        },
        plantName: {
          enum: ["Tomato", "Potato", "Rice", "Wheat"]
        },
        severity: {
          bsonType: "number",
          minimum: 1,
          maximum: 10
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
✔ Valid Insert
db.diagnosis.insertOne({
  diagId: "DIA101",
  plantName: "Tomato",
  severity: 7
})
❌ Invalid Insert
db.diagnosis.insertOne({
  diagId: "101",
  plantName: "Mango",
  severity: 20
})
📌 Output

Valid insert succeeds; invalid insert fails.

✅ Result

Advanced validation rules applied successfully.

🧪 EXPERIMENT – 5: Modify Validation
🎯 Aim

To modify validation rules in an existing collection.

💻 Query
db.createCollection("monitoring")

db.runCommand({
  collMod: "monitoring",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["plantId", "date", "healthStatus"],
      properties: {
        plantId: {
          bsonType: "string",
          pattern: "^PLT[0-9]{3}$"
        },
        date: { bsonType: "string" },
        healthStatus: {
          enum: ["Healthy", "Diseased", "Critical"]
        }
      }
    }
  },
  validationLevel: "moderate"
})
📌 Output
{ ok: 1 }
✅ Result

Validation rules updated successfully.

🧪 EXPERIMENT – 6: View Validation Rules
🎯 Aim

To view schema validation rules.

💻 Query
db.getCollectionInfos({ name: "diagnosis" })
📌 Output

Displays:

Required fields
Enum values
Pattern rules
Data constraints
✅ Result

Validation rules verified successfully.
