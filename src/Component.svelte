<script>
  import { getContext, createEventDispatcher } from "svelte"
  
  // Budibase SDK context
  const { styleable, Provider } = getContext("sdk")
  const component = getContext("component")
  const dispatch = createEventDispatcher()
  
  // Component props from schema
  export let dataProvider
  export let columns = 4
  export let cardHeight = "medium"
  export let showAddButton = true
  export let addButtonText = "Add New Vendor"
  export let onVendorClick
  export let onAddVendor
  export let defaultImage = ""
  export let showProfilePics = true
  export let cardStyle = "elevated"
  
  // Extract data from provider
  $: vendors = dataProvider?.rows || []
  $: loading = dataProvider?.loading || false
  $: error = dataProvider?.error || null
  
  // Calculate grid layout
  $: gridStyle = `grid-template-columns: repeat(${columns}, 1fr)`
  $: totalSlots = Math.ceil((vendors.length + (showAddButton ? 1 : 0)) / columns) * columns
  $: emptySlots = totalSlots - vendors.length - (showAddButton ? 1 : 0)
  
  // Provide context for child components
  $: dataContext = {
    vendors,
    loading,
    error,
    selectedVendor: null
  }
  
  function handleVendorClick(vendor) {
    dispatch('vendorClick', { vendor })
    if (onVendorClick) {
      onVendorClick({ vendor })
    }
  }
  
  function handleAddVendor() {
    dispatch('addVendor')
    if (onAddVendor) {
      onAddVendor()
    }
  }
  
  function getImageUrl(vendor) {
    if (!showProfilePics) return null
    return vendor.image_url || defaultImage || null
  }
  
  function getVendorType(vendor) {
    return vendor.vendor_type || "Vendor"
  }
  
  function formatPhoneNumber(phone) {
    if (!phone) return ""
    const phoneStr = phone.toString()
    if (phoneStr.length === 10) {
      return `(${phoneStr.slice(0,3)}) ${phoneStr.slice(3,6)}-${phoneStr.slice(6)}`
    }
    return phoneStr
  }
</script>

<div use:styleable={$component.styles} class="vendor-grid-container">
  <Provider data={dataContext}>
    {#if loading}
      <div class="loading-container">
        <div class="loading-spinner"></div>
        <p>Loading vendors...</p>
      </div>
    {:else if error}
      <div class="error-container">
        <p class="error-message">Error loading vendors: {error}</p>
      </div>
    {:else}
      <div class="vendor-grid" style={gridStyle}>
        <!-- Vendor Cards -->
        {#each vendors as vendor}
          <div 
            class="vendor-card {cardStyle} {cardHeight}"
            on:click={() => handleVendorClick(vendor)}
            on:keydown={(e) => e.key === 'Enter' && handleVendorClick(vendor)}
            role="button"
            tabindex="0"
            aria-label="View details for {vendor.vendor_name}"
          >
            <div class="card-content">
              <!-- Profile Picture Section -->
              {#if showProfilePics}
                <div class="image-section">
                  {#if getImageUrl(vendor)}
                    <img 
                      src={getImageUrl(vendor)} 
                      alt="Profile for {vendor.vendor_name}"
                      class="vendor-image"
                      loading="lazy"
                    />
                  {:else}
                    <div class="image-placeholder">
                      <svg viewBox="0 0 24 24" class="placeholder-icon">
                        <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/>
                      </svg>
                    </div>
                  {/if}
                </div>
              {/if}
              
              <!-- Vendor Information -->
              <div class="info-section">
                <h3 class="vendor-name">{vendor.vendor_name || "Unnamed Vendor"}</h3>
                
                {#if vendor.contact_name}
                  <p class="contact-name">{vendor.contact_name}</p>
                {/if}
                
                <p class="vendor-type">{getVendorType(vendor)}</p>
                
                {#if vendor.phone_number}
                  <p class="phone-number">{formatPhoneNumber(vendor.phone_number)}</p>
                {/if}
                
                {#if vendor.email}
                  <p class="email">{vendor.email}</p>
                {/if}
                
                {#if vendor.quoted_amount}
                  <p class="quoted-amount">${vendor.quoted_amount.toLocaleString()}</p>
                {/if}
                
                {#if vendor.is_booked}
                  <div class="booked-badge">Booked</div>
                {/if}
              </div>
            </div>
          </div>
        {/each}
        
        <!-- Add Vendor Button -->
        {#if showAddButton}
          <div 
            class="add-vendor-card {cardStyle} {cardHeight}"
            on:click={handleAddVendor}
            on:keydown={(e) => e.key === 'Enter' && handleAddVendor()}
            role="button"
            tabindex="0"
            aria-label="Add new vendor"
          >
            <div class="add-content">
              <div class="add-icon">
                <svg viewBox="0 0 24 24">
                  <path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/>
                </svg>
              </div>
              <p class="add-text">{addButtonText}</p>
            </div>
          </div>
        {/if}
        
        <!-- Empty slots for grid alignment -->
        {#each Array(emptySlots) as _}
          <div class="empty-slot"></div>
        {/each}
      </div>
    {/if}
    
    <!-- Slot for child components -->
    <slot />
  </Provider>
</div>

<style>
  .vendor-grid-container {
    width: 100%;
    padding: 1rem;
  }
  
  .vendor-grid {
    display: grid;
    gap: 1.5rem;
    width: 100%;
  }
  
  /* Loading States */
  .loading-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 3rem;
    text-align: center;
  }
  
  .loading-spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #3498db;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-bottom: 1rem;
  }
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  
  .error-container {
    padding: 2rem;
    text-align: center;
  }
  
  .error-message {
    color: #e74c3c;
    font-weight: 500;
  }
  
  /* Card Styles */
  .vendor-card, .add-vendor-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
  }
  
  /* Card Style Variants */
  .elevated {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .elevated:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  }
  
  .outlined {
    border: 2px solid #e1e5e9;
  }
  
  .outlined:hover {
    border-color: #3498db;
    transform: translateY(-2px);
  }
  
  .flat {
    background: #f8f9fa;
    border: 1px solid #dee2e6;
  }
  
  .flat:hover {
    background: #e9ecef;
  }
  
  /* Card Height Variants */
  .small {
    min-height: 180px;
  }
  
  .medium {
    min-height: 220px;
  }
  
  .large {
    min-height: 280px;
  }
  
  /* Card Content */
  .card-content {
    padding: 1.25rem;
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  
  .image-section {
    display: flex;
    justify-content: center;
    margin-bottom: 1rem;
  }
  
  .vendor-image {
    width: 64px;
    height: 64px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid #f1f3f4;
  }
  
  .image-placeholder {
    width: 64px;
    height: 64px;
    border-radius: 50%;
    background: #f1f3f4;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .placeholder-icon {
    width: 32px;
    height: 32px;
    fill: #9aa0a6;
  }
  
  .info-section {
    flex: 1;
    text-align: center;
  }
  
  .vendor-name {
    font-size: 1.1rem;
    font-weight: 600;
    color: #1a1a1a;
    margin: 0 0 0.5rem 0;
    line-height: 1.3;
  }
  
  .contact-name {
    font-size: 0.9rem;
    color: #5f6368;
    margin: 0 0 0.25rem 0;
  }
  
  .vendor-type {
    font-size: 0.85rem;
    color: #3498db;
    font-weight: 500;
    margin: 0 0 0.5rem 0;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  
  .phone-number {
    font-size: 0.8rem;
    color: #5f6368;
    margin: 0 0 0.25rem 0;
  }
  
  .email {
    font-size: 0.8rem;
    color: #5f6368;
    margin: 0 0 0.5rem 0;
    word-break: break-word;
  }
  
  .quoted-amount {
    font-size: 0.9rem;
    font-weight: 600;
    color: #27ae60;
    margin: 0.5rem 0 0 0;
  }
  
  .booked-badge {
    position: absolute;
    top: 0.75rem;
    right: 0.75rem;
    background: #27ae60;
    color: white;
    font-size: 0.7rem;
    font-weight: 600;
    padding: 0.25rem 0.5rem;
    border-radius: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  
  /* Add Vendor Card */
  .add-vendor-card {
    border: 2px dashed #cbd5e0;
    background: #f7fafc;
  }
  
  .add-vendor-card:hover {
    border-color: #3498db;
    background: #ebf8ff;
    transform: translateY(-2px);
  }
  
  .add-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
    padding: 2rem;
    text-align: center;
  }
  
  .add-icon {
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background: #3498db;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 1rem;
  }
  
  .add-icon svg {
    width: 24px;
    height: 24px;
    fill: white;
  }
  
  .add-text {
    font-size: 0.9rem;
    font-weight: 500;
    color: #4a5568;
    margin: 0;
  }
  
  .empty-slot {
    /* Invisible placeholder for grid alignment */
  }
  
  /* Responsive Design */
  @media (max-width: 1200px) {
    .vendor-grid {
      grid-template-columns: repeat(3, 1fr) !important;
    }
  }
  
  @media (max-width: 768px) {
    .vendor-grid {
      grid-template-columns: repeat(2, 1fr) !important;
      gap: 1rem;
    }
    
    .vendor-grid-container {
      padding: 0.75rem;
    }
    
    .card-content {
      padding: 1rem;
    }
    
    .vendor-name {
      font-size: 1rem;
    }
  }
  
  @media (max-width: 480px) {
    .vendor-grid {
      grid-template-columns: 1fr !important;
    }
  }
  
  /* Focus and Accessibility */
  .vendor-card:focus,
  .add-vendor-card:focus {
    outline: 2px solid #3498db;
    outline-offset: 2px;
  }
  
  .vendor-card:focus-visible,
  .add-vendor-card:focus-visible {
    outline: 2px solid #3498db;
    outline-offset: 2px;
  }
</style>