from fastapi import FastAPI
from pydantic import BaseModel
from datetime import datetime

app = FastAPI()

class LicenseRequest(BaseModel):
    license_key: str
    machine_id: str

LICENSES = {
    "TIMTY-GOLD-001": {
        "expires": "2026-12-31",
        "machine_id": None
    }
}

@app.post("/verify")
def verify(req: LicenseRequest):
    license_data = LICENSES.get(req.license_key)

    if not license_data:
        return {"status": "invalid", "reason": "License not found"}

    expiry = datetime.strptime(license_data["expires"], "%Y-%m-%d")
    if expiry < datetime.utcnow():
        return {"status": "expired", "reason": "Subscription expired"}

    if license_data["machine_id"] is None:
        license_data["machine_id"] = req.machine_id

    if license_data["machine_id"] != req.machine_id:
        return {"status": "invalid", "reason": "License used on another PC"}

    return {"status": "active"}
