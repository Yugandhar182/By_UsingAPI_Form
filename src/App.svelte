
<h1 style="color:red;">Enter details</h1>
<script>
	import { onMount } from 'svelte';
	import 'bootstrap/dist/css/bootstrap.min.css'
  
	let fullName = '';
	let email = '';
	let mobile = '';
	let cvFile = null;
	let fileContent = '';
	let fileName = '';
	let formErrors = {};
  
	async function handleFileChange(event) {
	  cvFile = event.target.files[0];
	  fileContent = await toBase64(cvFile);
	  fileName = cvFile.name;
	}
  
	async function uploadCV() {
	  formErrors = {};
  
	  if (!isValidName(fullName.trim())) {
		formErrors.fullName = 'Please enter a valid name with only letters.';
	  }
  
	  if (!email.trim()) {
		formErrors.email = 'Email is required.';
	  } else if (!isValidEmail(email)) {
		formErrors.email = 'Please enter a valid email address.';
	  }
  
	  if (!mobile.trim()) {
		formErrors.mobile = 'Mobile is required.';
	  } else if (!isValidMobile(mobile)) {
		formErrors.mobile = 'Please enter a valid  mobile number having 10 digits.';
	  }
  
	  if (!cvFile) {
		formErrors.cvFile = 'CV file is required.';
	  }
  
	  if (Object.keys(formErrors).length === 0) {
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
			resetForm();
			location.reload();
		  } else {
			console.error('Failed to upload CV');
		  }
		} catch (error) {
		  console.error('Error uploading CV:', error);
		}
	  }
	}
  
	function isValidName(name) {
	  const nameRegex = /^[A-Za-z]+$/;
	  return nameRegex.test(name);
	}
  
	function isValidEmail(email) {
	  // Basic email validation regex
	  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
	  return emailRegex.test(email);
	}
  
	function isValidMobile(mobile) {
	  // Basic mobile number validation regex
	  const mobileRegex = /^[0-9+]{10}$/;
	  return mobileRegex.test(mobile);
	}
  
	function toBase64(file) {
	  return new Promise((resolve, reject) => {
		const reader = new FileReader();
		reader.readAsDataURL(file);
		reader.onload = () => resolve(reader.result.split(',')[1]);
		reader.onerror = error => reject(error);
	  });
	}
  
	function resetForm() {
	  fullName = '';
	  email = '';
	  mobile = '';
	  cvFile = null;
	  fileContent = '';
	  fileName = '';
	  formErrors = {};
	}
  
	onMount(() => {
	  // Reset the file input after submission
	  const fileInput = document.getElementById('cvFileInput');
	  if (fileInput) {
		fileInput.value = '';
	  }
	});
  </script>
  
  <svelte:head>
	<link
	  rel="stylesheet"
	  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
	/>
  </svelte:head>
  
  <div class="form-group">
	<label for="firstName">First Name:</label>
	<input type="text" class="form-control" id="firstName" bind:value={fullName} placeholder="Enter your first name" />
	{#if formErrors.fullName}
	<p class="text-danger">{formErrors.fullName}</p>
	{/if}
  </div>
  
  <div class="form-group">
	<label for="email">Email:</label>
	<input type="email" class="form-control" id="email" bind:value={email} placeholder="Enter your email" />
	{#if formErrors.email}
	<p class="text-danger">{formErrors.email}</p>
	{/if}
  </div>
  
  <div class="form-group">
	<label for="mobile">Mobile:</label>
	<input type="tel" class="form-control" id="mobile" bind:value={mobile} placeholder="Enter your mobile number" />
	{#if formErrors.mobile}
	<p class="text-danger">{formErrors.mobile}</p>
	{/if}
  </div>
  
  <div class="form-group">
	<label for="cvFileInput">CV File:</label>
	<input type="file" class="form-control" accept=".pdf,.doc,.docx" id="cvFileInput" on:change={handleFileChange} />
	{#if formErrors.cvFile}
	<p class="text-danger">{formErrors.cvFile}</p>
	{/if}
  </div>
  
  <div class="form-group">
	<button class="btn btn-primary" on:click={uploadCV}>Submit</button>
  </div>
  
  <style>
	.text-danger {
	  color: red;
	}
  </style>
  