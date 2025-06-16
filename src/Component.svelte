<!-- Fixed Simplified Vendor Card Component for Budibase -->
<script>
  import { getContext, createEventDispatcher } from "svelte"
  
  // Required Budibase SDK context
  const { styleable, Provider } = getContext("sdk")
  const component = getContext("component")
  const dispatch = createEventDispatcher()
  
  // Props that need to be passed from Budibase
  export let dataProvider
  export let cardStyle = 'elevated'
  export let cardHeight = 'medium'
  export let onVendorClick
  export let defaultImage = 'https://pcmasfomyhqapaaaxzby.supabase.co/storage/v1/object/public/vendor-profile-pics/null_image.png'
  
  // Extract data from provider (Budibase pattern)
  $: vendors = dataProvider?.rows || []
  $: loading = dataProvider?.loading || false
  $: error = dataProvider?.error || null
  
  // Provide context for child components
  $: dataContext = {
    vendors,
    loading,
    error
  }
  
  function handleVendorClick(vendor) {
    dispatch('vendorClick', { vendor })
    if (onVendorClick) {
      onVendorClick({ vendor })
    }
  }
</script>

<!-- Required Budibase wrapper with styleable directive -->
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
      <div class="vendor-grid">
        {#each vendors as vendor, index}
          <div 
            class="vendor-card {cardStyle} {cardHeight} {vendor.is_booked ? 'booked' : 'not-booked'}"
            role="button"
            tabindex="0"
            on:click={() => handleVendorClick(vendor)}
            on:keydown={(e) => e.key === 'Enter' && handleVendorClick(vendor)}
            aria-label="View details for {vendor.vendor_name}"
          >
            <!-- Profile Picture -->
            <div class="vendor-image">
              <img 
                src={vendor.image_url || defaultImage || '/default-vendor.png'} 
                alt="{vendor.vendor_name} profile"
                loading="lazy"
                on:error={(e) => { e.target.src = defaultImage || '/default-vendor.png'; }}
              />
            </div>
            
            <!-- Vendor Information -->
            <div class="vendor-info">
              <h3 class="vendor-name">{vendor.vendor_name}</h3>
              <p class="contact-name">{vendor.contact_name || 'Contact not available'}</p>
              
              <!-- Booking Status Indicator -->
              <div class="booking-status {vendor.is_booked ? 'status-booked' : 'status-not-booked'}">
                {vendor.is_booked ? 'BOOKED' : 'NOT BOOKED'}
              </div>
            </div>
          </div>
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
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    width: 100%;
    max-width: 1400px;
    margin: 0 auto;
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
  
  .vendor-card {
    background: white;
    border-radius: 12px;
    cursor: pointer;
    transition: all 0.2s ease;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    position: relative;
    min-height: 240px;
    max-height: 400px;
  }
  
  /* Card Styles */
  .vendor-card.elevated {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .vendor-card.outlined {
    border: 1px solid #e0e0e0;
  }
  
  .vendor-card.flat {
    border: none;
    box-shadow: none;
  }
  
  /* Card Heights */
  .vendor-card.small {
    min-height: 200px;
    max-height: 280px;
  }
  
  .vendor-card.medium {
    min-height: 240px;
    max-height: 350px;
  }
  
  .vendor-card.large {
    min-height: 280px;
    max-height: 400px;
  }
  
  /* Booking Status Colors */
  .vendor-card.not-booked {
    border-left: 4px solid #f44336;
  }
  
  .vendor-card.booked {
    border-left: 4px solid #4caf50;
  }
  
  /* Hover Effects */
  .vendor-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
  }
  
  .vendor-card:focus {
    outline: 2px solid #2196f3;
    outline-offset: 2px;
  }
  
  /* Image Section */
  .vendor-image {
    flex: 1 1 auto;
    min-height: 120px;
    max-height: 200px;
    overflow: hidden;
    position: relative;
  }
  
  .vendor-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.2s ease;
  }
  
  .vendor-card:hover .vendor-image img {
    transform: scale(1.05);
  }
  
  /* Info Section */
  .vendor-info {
    padding: 16px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    background: white;
    flex: 0 0 auto;
    min-height: 100px;
  }
  
  .vendor-name {
    font-size: 1.1rem;
    font-weight: 600;
    margin: 0;
    color: #333;
    line-height: 1.3;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  .contact-name {
    font-size: 0.9rem;
    color: #666;
    margin: 0;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  /* Booking Status Badge */
  .booking-status {
    font-size: 0.75rem;
    font-weight: 600;
    padding: 8px 12px;
    border-radius: 4px;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-top: 8px;
    align-self: stretch;
  }
  
  .status-booked {
    background-color: #e8f5e8;
    color: #2e7d32;
  }
  
  .status-not-booked {
    background-color: #ffebee;
    color: #c62828;
  }
  
  /* Responsive Design */
  @media (max-width: 768px) {
    .vendor-grid {
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    }
    
    .vendor-card {
      height: auto;
      min-height: 200px;
    }
    
    .vendor-image {
      height: 140px;
    }
    
    .vendor-info {
      height: auto;
      padding: 12px;
    }
    
    .vendor-name {
      font-size: 1rem;
    }
    
    .contact-name {
      font-size: 0.85rem;
    }
  }
  
  @media (max-width: 480px) {
    .vendor-grid {
      grid-template-columns: 1fr;
    }
  }
</style>