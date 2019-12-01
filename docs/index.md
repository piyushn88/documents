# Notes

## Commands
**Python Server**

To share the file over network
 
* `python -m SimpleHTTPServer 8000`

**Mkdocs Commands**

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs help` - Print this help message.

**Core Service api**

* K8 Dashboard:	            `http://myminikube.info:30000/#!/pod?namespace=dev-core` 

* suite-ui:	                `https://myminikube.info:30070` 

* audit-service:	            `http://myminikube.info:30072` 

* core-authorization-service: `http://myminikube.info:30073` 

* budget service:             `http://myminikube.info:30074`

* configuration service:	    `http://myminikube.info:30075` 

* account service:	        `http://myminikube.info:30077/api-docs` 

* couchdb:	                `http://myminikube.info:30078/_utils` 

* help service:	            `http://myminikube.info:30079 `

* core-auth:	                `https://myminikube.info:30091`                    


**Orc api flow**
```buildoutcfg
orc/ext/ : 		
Ingest
	- Extracting : Type = job_state_transition
	- Ingested 	  : Type = job_state_transition

Enrich 
	- InvokeEnrich (onJobEvent)  : Type = invoke_operation
	- Enriching (onJobEvent)	: Type = job_state_transition
	- EnrichCompleted (onJobEvent) : Type = job_state_transition
	
	
orc/ : 		
	Extracting 		Type = job_state_transition
	Ingested		Type = job_state_transition
	Enriching		Type = job_state_transition
	EnrichCompleted		Type = job_state_transition
```
