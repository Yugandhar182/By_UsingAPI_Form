<script>
	import { onMount } from 'svelte';
	import 'bootstrap/dist/css/bootstrap.min.css';
  
	let formData = {
	  fullName: '',
	  email: '',
	  mobile: ''
	};
  
	let cvFile;
  
	function handleSubmit() {
	  const jsonData = {
		fullName: formData.fullName,
		email: formData.email,
		mobile: formData.mobile,
		fileContent: '', // Placeholder for file content (to be updated later)
		fileName: cvFile ? cvFile.name : '' // Get the file name if available
	  };
  
	  const formData = new FormData();
	  formData.append('data', JSON.stringify(jsonData));
	  formData.append('cv', cvFile);
  
	  fetch('https://api.recruitly.io/api/cvsubmit/bytes?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77', {
		method: 'POST',
		body: formData
	  })
		.then(response => response.json())
		.then(data => {
		  // Handle the response data
		  console.log(data);
		  // Perform additional actions with the response, if needed
		  // For example, display a success message to the user
		  alert('CV submitted successfully!');
		})
		.catch(error => {
		  // Handle any errors
		  console.error(error);
		  // Display an error message to the user
		  alert('An error occurred while submitting the CV. Please try again later.');
		});
	}
  
	function handleCVUpload(event) {
	  cvFile = event.target.files[0];
	}
  
	onMount(() => {
	  // Perform initialization or other tasks after the component mounts
	});
  </script>
  
  <main>
	<form on:submit={handleSubmit} class="container">
	  <div class="form-group">
		<label for="fullName">Full Name</label>
		<input type="text" id="fullName" name="fullName" class="form-control" bind:value={formData.fullName} />
	  </div>
  
	  <div class="form-group">
		<label for="email">Email</label>
		<input type="email" id="email" name="email" class="form-control" bind:value={formData.email} />
	  </div>
  
	  <div class="form-group">
		<label for="mobile">Mobile</label>
		<input type="tel" id="mobile" name="mobile" class="form-control" bind:value={formData.mobile} />
	  </div>
  
	  <div class="form-group">
		<label for="cv">Upload CV</label>
		<input type="file" id="cv" name="cv" class="form-control-file" on:change={handleCVUpload} />
	  </div>
  
	  <button type="submit" class="btn btn-primary">Submit</button>
	</form>
  </main>
  