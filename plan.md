```markdown
# Detailed Implementation Plan for Incorporating Spanish Pricing Data

This plan updates the marketplace to include the extended Spanish pricing information for streaming and digital services. The new data will be integrated into the data module and reflected in the UI components. All changes follow best practices, include error handling, and use modern styling with typography, spacing, and layout only.

---

## 1. Update Data Types

### File: src/lib/types.ts
- **Modification:**  
  - Update the `duration` union in the `AccountPackage` interface to include a new value `"6months"`.  
  - *Change:*  
    ```typescript
    export interface AccountPackage {
      id: string;
      serviceId: string;
      type: 'individual' | 'family' | 'premium';
      duration: '1month' | '3months' | '6months' | '1year';
      originalPrice: number;
      salePrice: number;
      features: string[];
      popular?: boolean;
    }
    ```
- **Error Handling:**  
  - No runtime errors expected; ensure TypeScript compiles without errors.

---

## 2. Extend and Update Data

### File: src/lib/data.ts
- **Update Streaming Services Array:**  
  - **For Existing Services:**  
    - Update Netflix entry by enhancing the description (e.g., "Suscripciones en español con nuevos precios").  
    - Leave Disney+, HBO Max, Hulu, Prime Video, and Apple TV+ entries; they will get new packages through accountPackages.
  - **Add New Service Entries:**  
    - Append new items using unique `id` values (e.g., `"max"`, `"paramount"`, `"crunchyroll"`, `"universal"`) with appropriate names, simple text logos (like text emoticons), color codes, and brief descriptive texts.
    - Add entries also for non-traditional streaming services (Plex, Mubi, NBA, WWE, MLB, Gaia, VIX Premium, Viki, Flujo TV, IPTV, Emby, Porhub, Metegol, IPTV Completa, Win Online, Claro Video, DirectTV variants, Jellyfin, Telelatino) and digital/music/gaming services (Spotify, YouTube, Deezer, Canva, Game Pass, CapCut, Chat-Gpt, Duolingo).
  - **Example New Service Entry:**  
    ```typescript
    {
      id: 'paramount',
      name: 'Paramount+',
      logo: '📡',
      color: '#003366',
      description: 'Contenido exclusivo y de alta calidad en español'
    }
    ```
- **Update Account Packages Array:**  
  - **For "Pantallas individuales":**  
    - For Netflix, add three packages:
      - `netflix-standard-1m`: salePrice 8550, duration `'1month'`, features like "Pantalla individual, Calidad HD".
      - `netflix-extra-1m`: salePrice 10000, duration `'1month'`, features "Contenido adicional extra".
      - `netflix-internacional-1m`: salePrice 9800, duration `'1month'`, features "Acceso internacional".
  - **For Disney+:**
    - Add:
      - `disney-standard-1m`: salePrice 3500, duration `'1month'`, features "Acceso estándar".
      - `disney-premium-1m`: salePrice 4800, duration `'1month'`, features "Incluye Star+ y ESPN".
      - `disney-combo-1m`: salePrice 4000, duration `'1month'`, features "Combo Disney+ y Star+".
  - **For Other Services:**  
    - For each new service (Max, Prime, Paramount+, Crunchyroll, etc.), create a respective package.  
    - **Example for Prime Video:**  
      ```typescript
      {
        id: 'prime-1m',
        serviceId: 'prime',
        type: 'individual',
        duration: '1month',
        originalPrice: 2800,
        salePrice: 2800,
        features: ['Acceso individual', 'Contenido exclusivo']
      },
      {
        id: 'prime-6m',
        serviceId: 'prime',
        type: 'individual',
        duration: '6months',
        originalPrice: 7000,
        salePrice: 7000,
        features: ['Suscripción de 6 meses', 'Ahorro significativo']
      }
      ```
    - Repeat similar structure for other services using the provided pricing (e.g., Plex, NBA, Spotify, Duolingo, etc.).
- **Best Practices:**  
  - Ensure numeric pricing values have no formatting issues.
  - Add basic features as arrays of strings (e.g., "1 Pantalla", "HD Quality") for clarity.
  - Validate that each `accountPackages` item has a matching `serviceId` in `streamingServices`.

---

## 3. UI Component Adjustments

### File: src/components/marketplace/StreamingServiceCard.tsx
- **Changes:**  
  - Update the card layout to list multiple packages with their price and duration details.
  - Use modern typography and spacing (for example, display the package name, price in a bold font, and duration in a secondary text style).
  - Ensure error boundaries: if a service has no packages, display an informative message.
- **Design Consideration:**  
  - Use plain text for prices and a colored background based on `service.color` for visual emphasis.
  
### File: src/components/marketplace/ServiceSidebar.tsx
- **Changes:**  
  - Ensure the sidebar renders all streaming services, including the newly added ones.
  - Improve responsiveness for a larger list by using a scrollable container.
  
### File: src/components/cart/ShoppingCart.tsx
- **Changes:**  
  - Verify that cart item display shows updated pricing values.
  - Confirm that update and removal functions continue to work with the expanded list.
  - Apply modern spacing and clear typography to present the cart summary.

---

## 4. Integration in the Main Page

### File: src/app/page.tsx
- **Verification and Minor Adjustments:**  
  - Confirm that the filtering logic (by `selectedService` and search query) works seamlessly with the extended list from `streamingServices` and `accountPackages`.
  - Ensure the “Add to Cart” functionality properly locates new packages using their updated IDs.
  - Check that featured content and “See more” buttons remain styled in a modern, consistent manner.
  
---

## 5. Testing and Error Handling

- **Testing Approach:**  
  - Manually verify that each new service appears in the sidebar and that its corresponding packages are displayed on its card.
  - Simulate adding packages to the cart; ensure that undefined packages or mismatches are caught by the existing null-checks.
  - Adjust responsive breakpoints if necessary to accommodate the increased data.
- **Error Handling:**  
  - In `handleAddToCart`, if either the package or service is not found, the function should exit silently (or later display a user-friendly error message).
  - All UI components include fallback styling to maintain layout even if an image or icon fails to load.

---

## 6. Final Steps

- Update version control with a commit message such as “Update Spanish pricing data and add extended digital services.”
- Perform local testing across different screen sizes.
