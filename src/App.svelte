<script>
	let fullName = '';
	let email = '';
	let mobile = '';
	let cvFile = null;
	let fileContent = '';
	let fileName = '';
  
	async function handleFileChange(event) {
	  cvFile = event.target.files[0];
	  fileContent = await toBase64(cvFile);
	  fileName = cvFile.name;
	}
  
	async function uploadCV() {
	  if (cvFile && fileContent && fileName) {
		const postData = {
		  fullName,
		  email,
		  mobile,
		  fileContent,
		  fileName
		};
  
		try {
		  const response = await fetch('https://api.recruitly.io/api/cvsubmit/bytes?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77', {
			method: 'POST',
			headers: {
			  'Content-Type': 'application/json'
			},
			body: JSON.stringify(postData)
		  });
  
		  // Handle the response as needed
		  if (response.ok) {
			console.log('CV uploaded successfully');
		  } else {
			console.error('Failed to upload CV');
		  }
		} catch (error) {
		  console.error('Error uploading CV:', error);
		}
	  }
	}
  
	function toBase64(file) {
	  return new Promise((resolve, reject) => {
		const reader = new FileReader();
		reader.readAsDataURL(file);
		reader.onload = () => resolve(reader.result.split(',')[1]);
		reader.onerror = error => reject(error);
	  });
	}
  </script>
  
  <style>
	@import 'bootstrap/dist/css/bootstrap.min.css';
  </style>
  
  <main class="container">
	<label class="form-label">
	  Full Name:
	  <input type="text" class="form-control" bind:value={fullName} />
	</label>
	<label class="form-label">
	  Email:
	  <input type="email" class="form-control" bind:value={email} />
	</label>
	<label class="form-label">
	  Mobile:
	  <input type="tel" class="form-control" bind:value={mobile} />
	</label>
	<input type="file" class="form-control" accept=".pdf,.doc,.docx" on:change={handleFileChange} />
	<button class="btn btn-primary" on:click={uploadCV}>Submit</button>
  </main>
  