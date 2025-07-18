<template>
  <div class="demo-panel">
    <h2>Panel 3 - Form Demo</h2>
    <p>This panel contains a form with various inputs. All form data is preserved when switching tabs.</p>
    
    <div class="form-section">
      <form @submit.prevent="submitForm">
        <div class="form-group">
          <label for="name">Name:</label>
          <input 
            id="name"
            v-model="formData.name" 
            type="text" 
            placeholder="Enter your name"
            class="form-input"
          />
        </div>
        
        <div class="form-group">
          <label for="email">Email:</label>
          <input 
            id="email"
            v-model="formData.email" 
            type="email" 
            placeholder="Enter your email"
            class="form-input"
          />
        </div>
        
        <div class="form-group">
          <label for="age">Age:</label>
          <input 
            id="age"
            v-model.number="formData.age" 
            type="number" 
            placeholder="Enter your age"
            class="form-input"
          />
        </div>
        
        <div class="form-group">
          <label for="country">Country:</label>
          <select id="country" v-model="formData.country" class="form-select">
            <option value="">Select a country</option>
            <option value="us">United States</option>
            <option value="uk">United Kingdom</option>
            <option value="ca">Canada</option>
            <option value="au">Australia</option>
            <option value="de">Germany</option>
            <option value="fr">France</option>
            <option value="jp">Japan</option>
            <option value="cn">China</option>
          </select>
        </div>
        
        <div class="form-group">
          <label>Interests:</label>
          <div class="checkbox-group">
            <label class="checkbox-item">
              <input type="checkbox" v-model="formData.interests" value="technology" />
              Technology
            </label>
            <label class="checkbox-item">
              <input type="checkbox" v-model="formData.interests" value="sports" />
              Sports
            </label>
            <label class="checkbox-item">
              <input type="checkbox" v-model="formData.interests" value="music" />
              Music
            </label>
            <label class="checkbox-item">
              <input type="checkbox" v-model="formData.interests" value="travel" />
              Travel
            </label>
          </div>
        </div>
        
        <div class="form-group">
          <label for="bio">Bio:</label>
          <textarea 
            id="bio"
            v-model="formData.bio" 
            placeholder="Tell us about yourself..."
            class="form-textarea"
            rows="4"
          ></textarea>
        </div>
        
        <button type="submit" class="btn btn-primary">Submit Form</button>
      </form>
      
      <div v-if="submitted" class="success-message">
        <h3>Form Submitted Successfully!</h3>
        <pre>{{ JSON.stringify(formData, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'

const formData = reactive({
  name: '',
  email: '',
  age: null as number | null,
  country: '',
  interests: [] as string[],
  bio: ''
})

const submitted = ref(false)

const submitForm = () => {
  submitted.value = true
  console.log('Form submitted:', formData)
}
</script>

<style scoped>
.demo-panel {
  padding: 20px;
}

.form-section {
  margin: 20px 0;
  padding: 20px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 500;
  color: #374151;
}

.form-input,
.form-select,
.form-textarea {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.2s ease;
}

.form-input:focus,
.form-select:focus,
.form-textarea:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

.checkbox-group {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
}

.checkbox-item {
  display: flex;
  align-items: center;
  gap: 5px;
  font-weight: normal;
  cursor: pointer;
}

.checkbox-item input[type="checkbox"] {
  width: auto;
  margin: 0;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
  font-size: 14px;
}

.btn-primary {
  background-color: #2563eb;
  color: white;
}

.btn:hover {
  opacity: 0.9;
}

.success-message {
  margin-top: 20px;
  padding: 20px;
  background-color: #d1fae5;
  border: 1px solid #10b981;
  border-radius: 8px;
}

.success-message h3 {
  color: #065f46;
  margin-bottom: 10px;
}

.success-message pre {
  background-color: #f3f4f6;
  padding: 10px;
  border-radius: 4px;
  font-size: 12px;
  overflow-x: auto;
}
</style>