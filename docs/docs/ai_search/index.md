# index

## AI Docs Search

Search through our documentation using AI-powered natural language queries.

&#x20;Search\
window.addEventListener('load', function() {\
&#x20;   function extractText(responseText) {\
&#x20;       try {\
&#x20;           console.log('responseText: ', responseText);\
&#x20;           const results = JSON.parse(responseText);\
&#x20;           const msg = results.message;\
\
&#x20;           if (!msg || msg.trim() === '') {\
&#x20;               return "No results found";\
&#x20;           }\
&#x20;           return msg;\
&#x20;       } catch (error) {\
&#x20;           console.error('Error parsing results:', error);\
&#x20;           throw new Error("Failed parsing response message");\
&#x20;       }\
&#x20;   }\
\
&#x20;   function displayResults(msg) {\
&#x20;       const resultsContainer = document.getElementById('results');\
&#x20;       const spinner = document.getElementById('spinner');\
&#x20;       const searchContainer = document.querySelector('.search-container');\
\
&#x20;       // Hide spinner\
&#x20;       spinner.style.display = 'none';\
\
&#x20;       // Scroll to search bar\
&#x20;       searchContainer.scrollIntoView({ behavior: 'smooth', block: 'start' });\
\
&#x20;       try {\
&#x20;           marked.setOptions({\
&#x20;               breaks: true,\
&#x20;               gfm: true,\
&#x20;               headerIds: false,\
&#x20;               sanitize: false\
&#x20;           });\
\
&#x20;           const htmlContent = marked.parse(msg);\
\
&#x20;           resultsContainer.className = 'markdown-content';\
&#x20;           resultsContainer.innerHTML = htmlContent;\
\
&#x20;           // Scroll after content is rendered\
&#x20;           setTimeout(() => {\
&#x20;               const searchContainer = document.querySelector('.search-container');\
&#x20;               const offset = 55; // Offset from top in pixels\
&#x20;               const elementPosition = searchContainer.getBoundingClientRect().top;\
&#x20;               const offsetPosition = elementPosition + window.pageYOffset - offset;\
\
&#x20;               window.scrollTo({\
&#x20;                   top: offsetPosition,\
&#x20;                   behavior: 'smooth'\
&#x20;               });\
&#x20;           }, 100);\
&#x20;       } catch (error) {\
&#x20;           console.error('Error parsing results:', error);\
&#x20;           resultsContainer.innerHTML = '\<div class="error-message">Cannot process results\</div>';\
&#x20;       }\
&#x20;   }\
\
&#x20;   async function performSearch() {\
&#x20;       const searchInput = document.getElementById('searchInput');\
&#x20;       const resultsContainer = document.getElementById('results');\
&#x20;       const spinner = document.getElementById('spinner');\
&#x20;       const searchTerm = searchInput.value.trim();\
\
&#x20;       if (!searchTerm) {\
&#x20;           resultsContainer.innerHTML = '\<div class="error-message">Please enter a search term\</div>';\
&#x20;           return;\
&#x20;       }\
\
&#x20;       // Show spinner, clear results\
&#x20;       spinner.style.display = 'flex';\
&#x20;       resultsContainer.innerHTML = '';\
\
&#x20;       try {\
&#x20;           const data = {\
&#x20;               "query": searchTerm\
&#x20;           };\
\
&#x20;           const options = {\
&#x20;               method: 'POST',\
&#x20;               headers: {\
&#x20;                   'accept': 'text/plain',\
&#x20;                   'content-type': 'application/json',\
&#x20;               },\
&#x20;               body: JSON.stringify(data)\
&#x20;           };\
\
&#x20;           //const API\_ENDPOINT = 'http://0.0.0.0:3000/api/v1/docs\_help';\
&#x20;           const API\_ENDPOINT = 'https://help.merge.qodo.ai/api/v1/docs\_help';\
\
&#x20;           const response = await fetch(API\_ENDPOINT, options);\
&#x20;           const responseText = await response.text();\
&#x20;           const msg = extractText(responseText);\
\
&#x20;           if (!response.ok) {\
&#x20;               throw new Error(\`An error (${response.status}) occurred during search: "${msg}"\`);\
&#x20;           }\
&#x20;\
&#x20;           displayResults(msg);\
&#x20;       } catch (error) {\
&#x20;           spinner.style.display = 'none';\
&#x20;           const errorDiv = document.createElement('div');\
&#x20;           errorDiv.className = 'error-message';\
&#x20;           errorDiv.textContent = error instanceof Error ? error.message : String(error);\
&#x20;           resultsContainer.replaceChildren(errorDiv);\
&#x20;       }\
&#x20;   }\
\
&#x20;   // Add event listeners\
&#x20;   const searchButton = document.getElementById('searchButton');\
&#x20;   const searchInput = document.getElementById('searchInput');\
\
&#x20;   if (searchButton) {\
&#x20;       searchButton.addEventListener('click', performSearch);\
&#x20;   }\
\
&#x20;   if (searchInput) {\
&#x20;       searchInput.addEventListener('keypress', function(e) {\
&#x20;           if (e.key === 'Enter') {\
&#x20;               performSearch();\
&#x20;           }\
&#x20;       });\
&#x20;   }\
});
