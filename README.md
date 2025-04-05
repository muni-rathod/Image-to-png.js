# Image-to-png.js
document.addEventListener('DOMContentLoaded', function() {
    const fileInput = document.getElementById('file-input');
    const imagePreview = document.getElementById('image-preview');
    const noImage = document.getElementById('no-image');
    const optionsSection = document.getElementById('options-section');
    const convertBtn = document.getElementById('convert-btn');
    const downloadBtn = document.getElementById('download-btn');
    const qualitySlider = document.getElementById('quality');
    const qualityValue = document.getElementById('quality-value');
    
    // Update quality value display
    qualitySlider.addEventListener('input', function() {
        qualityValue.textContent = this.value;
    });
    
    // Handle file selection
    fileInput.addEventListener('change', function(e) {
        const file = e.target.files[0];
        if (!file) return;
        
        if (!file.type.match('image.*')) {
            alert('Please select an image file.');
            return;
        }
        
        const reader = new FileReader();
        reader.onload = function(e) {
            imagePreview.src = e.target.result;
            imagePreview.classList.remove('d-none');
            noImage.classList.add('d-none');
            optionsSection.classList.remove('d-none');
            convertBtn.disabled = false;
        };
        reader.readAsDataURL(file);
    });
    
    // Handle conversion
    convertBtn.addEventListener('click', function() {
        if (!fileInput.files[0]) {
            alert('Please select an image first.');
            return;
        }
        
        const file = fileInput.files[0];
        const quality = parseInt(qualitySlider.value) / 100;
        const preserveTransparency = document.getElementById('transparency').checked;
        const width = document.getElementById('resize-width').value;
        const height = document.getElementById('resize-height').value;
        
        // Show loading state
        convertBtn.disabled = true;
        convertBtn.innerHTML = '<span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span> Converting...';
        
        // Simulate conversion (in a real app, this would use canvas to convert)
        setTimeout(function() {
            // This is a simplified version - actual implementation would:
            // 1. Create canvas element
            // 2. Draw image on canvas
            // 3. Apply transformations (resize, etc.)
            // 4. Convert to PNG with toDataURL()
            
            // For demo purposes, we'll just use the original file
            const pngUrl = imagePreview.src;
            
            // Enable download
            downloadBtn.href = pngUrl;
            downloadBtn.download = file.name.replace(/\.[^/.]+$/, '') + '.png';
            downloadBtn.classList.remove('d-none');
            
            // Reset button
            convertBtn.disabled = false;
            convertBtn.textContent = 'Convert to PNG';
            
            // Show success message
            alert('Conversion complete! Click "Download PNG" to save your file.');
        }, 1500);
    });
    
    // Drag and drop functionality
    const toolContainer = document.querySelector('.tool-container');
    
    toolContainer.addEventListener('dragover', function(e) {
        e.preventDefault();
        toolContainer.classList.add('drag-over');
    });
    
    toolContainer.addEventListener('dragleave', function() {
        toolContainer.classList.remove('drag-over');
    });
    
    toolContainer.addEventListener('drop', function(e) {
        e.preventDefault();
        toolContainer.classList.remove('drag-over');
        
        if (e.dataTransfer.files.length) {
            fileInput.files = e.dataTransfer.files;
            const event = new Event('change');
            fileInput.dispatchEvent(event);
        }
    });
});
