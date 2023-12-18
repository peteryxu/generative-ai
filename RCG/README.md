# ALL

## python package installation
pip install jupyterlab

## Vertex AI:  Vector Search 


### curl

curl -X POST -H "Authorization: Bearer $(gcloud auth print-access-token)" \
"https://1815468942.us-central1-754566683040.vdb.vertexai.goog/v1/projects/754566683040/locations/us-central1/indexEndpoints/3647146040030658560:findNeighbors" \
-d '{deployedIndexId:"embvs_tutorial_deployed_12152009", "queries":[{datapoint:{"featureVector":"<FEATURE_VECTOR>"}}], returnFullDatapoint:false}'

### Python

`
from google.cloud import aiplatform_v1

# Set variables for the current deployed index.
API_ENDPOINT="1815468942.us-central1-754566683040.vdb.vertexai.goog"
INDEX_ENDPOINT="projects/754566683040/locations/us-central1/indexEndpoints/3647146040030658560"
DEPLOYED_INDEX_ID="embvs_tutorial_deployed_12152009"

# Configure Vector Search client
client_options = {
  "api_endpoint": API_ENDPOINT
}
vector_search_client = aiplatform_v1.MatchServiceClient(
  client_options=client_options,
)

# Build FindNeighborsRequest object
datapoint = aiplatform_v1.IndexDatapoint(
  feature_vector="<FEATURE_VECTOR>"
)
query = aiplatform_v1.FindNeighborsRequest.Query(
  datapoint=datapoint,
  # The number of nearest neighbors to be retrieved
  neighbor_count=10
)
request = aiplatform_v1.FindNeighborsRequest(
  index_endpoint=INDEX_ENDPOINT,
  deployed_index_id=DEPLOYED_INDEX_ID,
  # Request can have multiple queries
  queries=[query],
  return_full_datapoint=False,
)

# Execute the request
response = vector_search_client.find_neighbors(request)

# Handle the response
print(response)


`