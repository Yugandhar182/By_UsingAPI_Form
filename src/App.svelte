<script>
	let cvFile = null;
  
	function handleFileChange(event) {
	  cvFile = event.target.files[0];
	}
  
	async function uploadCV() {
	  if (cvFile) {
		const formData = new FormData();
		formData.append('file', cvFile);
  
		const response = await fetch('https://api.recruitly.io/api/cvsubmit/multipart?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77', {
		  method: 'POST',
		  body: formData
		});
  
		// Handle the response as needed
		if (response.ok) {
		  console.log('CV uploaded successfully');
		} else {
		  console.error('CV upload failed');
		}
	  }
	}
  </script>
  
  <main>
	<h1>Upload CV</h1>
	<input type="file" accept=".pdf,.doc,.docx" on:change={handleFileChange} />
	<button on:click={uploadCV}>Upload</button>
  </main>
  