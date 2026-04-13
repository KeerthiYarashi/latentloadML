# API Specification — LatentML v2.0

## Base URL
http://localhost:8000

## Endpoints

### POST /api/request  (UNCHANGED)
Trigger a new routing request.
Request: No body needed.
Response:
{
  "selected_server": "ServerB",
  "score": 38.5,
  "weights": {"w1":1.0,"w2":0.5,"w3":10.0,"w4":2.0},
  "all_scores": [
    {"server":"ServerA","latency":15,"load":80,"queue_depth":4,"ml_risk":0.9,"status":"DOWN","score":null},
    {"server":"ServerB","latency":22,"load":20,"queue_depth":1,"ml_risk":0.1,"status":"HEALTHY","score":38.5},
    ...
  ]
}

### GET /api/servers  (UPDATED)
Response:
{
  "servers": {
    "ServerA": {"load":80,"history":[60,65,70,75,80],"queue_depth":4,"status":"DOWN"},
    "ServerB": {"load":20,"history":[25,22,20,20,20],"queue_depth":1,"status":"HEALTHY"}
  }
}

### GET /api/history/{server_name}  (UNCHANGED)
Response:
{
  "server": "ServerA",
  "history": [40, 50, 60, 70, 80]
}

### GET /api/compare  (UNCHANGED)
Response:
{
  "latentml_selected": "ServerB",
  "latentml_score": 38.5,
  "roundrobin_selected": "ServerA",
  "roundrobin_score": 64.0,
  "latentml_wins": true
}

### GET /api/weights  (NEW)
Get current dynamic weights.
Response:
{
  "w1": 1.0,
  "w2": 0.5,
  "w3": 10.0,
  "w4": 2.0
}

### GET /api/log  (NEW)
Get last 20 routing decisions.
Response:
{
  "log": [
    {"request_id":1,"selected_server":"ServerB","score":38.5,"timestamp":"..."},
    ...
  ]
}

### POST /api/simulate?requests=20  (NEW)
Fire N back-to-back routing requests for stress testing.
Response:
{
  "total_requests": 20,
  "server_selection_count": {
    "ServerB": 10,
    "ServerE": 7,
    "ServerD": 3
  },
  "average_score": 41.2,
  "servers_degraded": ["ServerC"],
  "servers_downed": ["ServerA"]
}

### GET /  (UNCHANGED)
Response: {"status": "LatentML running", "version": "2.0"}
