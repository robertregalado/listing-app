## Home Renting Listing App 

A simple home renting listing application built using the Angular framework and JSON Server. The application features a listing of available rental units, including details such as location, state, photo, Wi-Fi availability, and laundry facilities.
![Home Renting](https://github.com/robertregalado/listing-app/blob/master/snapshot.png)
### Features

- **Units Available**: Display the number of rental units available.
- **Location**: Show the location of each rental unit.
- **State**: Show the state where the rental unit is located.
- **Photo**: Display a photo of each rental unit.
- **Wi-Fi**: Indicate whether Wi-Fi is available in each rental unit.
- **Laundry**: Indicate whether laundry facilities are available in each rental unit.

### Technologies Used

- **Angular**: A platform for building web applications.
- **JSON Server**: A simple backend for quick prototyping that provides a full fake REST API.

### Project Structure

```
home-listing-app/
├── src/
│   ├── app/
│   │   ├── details/
│   │   │   ├── details.component.css
│   │   │   ├── details.component.ts
│   │   ├── home/
│   │   │   ├── home.component.css
│   │   │   ├── home.component.ts 
│   │   ├── housing-location/
│   │   │   ├── housing-location.component.css
│   │   │   ├── housing-location.component.ts
│   │   ├── app.component.css
│   │   ├── app.component.ts
│   │   ├── app.config.ts
│   │   ├── housing-location.ts
│   │   ├── housing.service.ts
│   │   ├── routes.css
│   ├── assets/
│   │   ├── angular.svg
│   │   ├── location-pin.svg
│   │   ├── logo.svg
│   ├── favicon.ico/
│   ├── index.html/
│   ├── main.ts/
│   ├── style.css/
├── angular.json
├── package.json
├── package-lock.json
├── package.json.template
├── db.json
├── .gitignore
├── config.json
├── BUILD.bazel
├── tsconfig.app.json
├── tsconfig.json
```

### Installation and Setup

1. **Clone the Repository:**

   ```sh
   git clone https://github.com/your-username/listing-app.git
   cd listing-app
   ```

2. **Install Dependencies:**

   ```sh
   npm install
   ```

3. **Setup JSON Server:**

   Create a `db.json` file in the root directory with the following content:

   ```json
   {
     "locations": [
        {
            "id": 0,
            "name": "Acme Fresh Start Housing",
            "city": "Chicago",
            "state": "IL",
            "photo": "https://angular.io/assets/images/tutorials/faa/bernard-hermant-CLKGGwIBTaY-unsplash.jpg",
            "availableUnits": 4,
            "wifi": true,
            "laundry": true
          },
          {
            "id": 1,
            "name": "A113 Transitional Housing",
            "city": "Santa Monica",
            "state": "CA",
            "photo": "https://angular.io/assets/images/tutorials/faa/brandon-griggs-wR11KBaB86U-unsplash.jpg",
            "availableUnits": 0,
            "wifi": false,
            "laundry": true
          },
     ]
   }
   ```

   Start JSON Server:

   ```sh
   npx json-server --watch db.json --port 3000
   ```

4. **Run the Angular Application:**

   ```sh
   ng serve
   ```

   Open your browser and navigate to `http://localhost:4200`.

### Angular Components

#### Home Component

**home.component.ts**

```typescript
import { Component, inject } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HousingLocationComponent } from '../housing-location/housing-location.component';
import { HousingLocation } from '../housing-location';
import { HousingService } from '../housing.service';

@Component({
  selector: 'app-home',
  standalone: true,
  imports: [
    CommonModule,
    HousingLocationComponent
  ],
  template: `
    <section>
      <form>
        <input type="text" placeholder="Filter by city" #filter>
        <button class="primary" type="button" (click)="filterResults(filter.value)">Search</button>
      </form>
    </section>
    <section class="results">
      <app-housing-location *ngFor="let housingLocation of filteredLocationList" [housingLocation]="housingLocation"></app-housing-location>
    </section>
    `,
  styleUrls: ['./home.component.css'],
})

export class HomeComponent {
      housingLocationList: HousingLocation[] = [];
      housingService: HousingService = inject(HousingService);
      filteredLocationList: HousingLocation[] = [];

      constructor() {
        this.housingService.getAllHousingLocations().then((housingLocationList: HousingLocation[]) => {
          this.housingLocationList = housingLocationList;
          this.filteredLocationList = housingLocationList;
        });
      }

      filterResults(text: string) {
        if (!text) this.filteredLocationList = this.housingLocationList;

        this.filteredLocationList = this.housingLocationList.filter(
          housingLocation => housingLocation?.city.toLowerCase().includes(text.toLowerCase())
        );
      }
}
```

**home.component.ts**

```typescript
import { Component, inject } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HousingLocationComponent } from '../housing-location/housing-location.component';
import { HousingLocation } from '../housing-location';
import { HousingService } from '../housing.service';

@Component({
  selector: 'app-home',
  standalone: true,
  imports: [
    CommonModule,
    HousingLocationComponent
  ],
  template: `
    <section>
      <form>
        <input type="text" placeholder="Filter by city" #filter>
        <button class="primary" type="button" (click)="filterResults(filter.value)">Search</button>
      </form>
    </section>
    <section class="results">
      <app-housing-location *ngFor="let housingLocation of filteredLocationList" [housingLocation]="housingLocation"></app-housing-location>
    </section>
    `,
  styleUrls: ['./home.component.css'],
})

export class HomeComponent {
      housingLocationList: HousingLocation[] = [];
      housingService: HousingService = inject(HousingService);
      filteredLocationList: HousingLocation[] = [];

      constructor() {
        this.housingService.getAllHousingLocations().then((housingLocationList: HousingLocation[]) => {
          this.housingLocationList = housingLocationList;
          this.filteredLocationList = housingLocationList;
        });
      }

      filterResults(text: string) {
        if (!text) this.filteredLocationList = this.housingLocationList;

        this.filteredLocationList = this.housingLocationList.filter(
          housingLocation => housingLocation?.city.toLowerCase().includes(text.toLowerCase())
        );
      }
}

```

#### Housing Location Component

**housing-location.component.ts**

```typescript
import { Component, Input } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HousingLocation } from '../housing-location';
import { RouterModule } from '@angular/router';

@Component({
  selector: 'app-housing-location',
  standalone: true,
  imports: [CommonModule, RouterModule],
  template: `
    <section class="listing">
      <img class="listing-photo" [src]="housingLocation.photo" alt="Exterior photo of {{housingLocation.name}}">
      <h2 class="listing-heading">{{ housingLocation.name }}</h2>
      <p class="listing-location">{{ housingLocation.city}}, {{ housingLocation.state }}</p>
      <a [routerLink]="['/details', housingLocation.id]">Learn More</a>
    </section>
  `,
  styleUrls: ['./housing-location.component.css'],
})

export class HousingLocationComponent {

  @Input() housingLocation!: HousingLocation;

}

```

**housing-location.component.html**

```html
<div class="housing-location">
  <img [src]="housingLocation.photo" alt="Photo of {{ housingLocation.city }}">
  <h2>{{ housingLocation.city }}, {{ housingLocation.state }}</h2>
  <p>Wi-Fi: {{ housingLocation.wifi ? 'Available' : 'Not Available' }}</p>
  <p>Laundry: {{ housingLocation.laundry ? 'Available' : 'Not Available' }}</p>
</div>
```

### Services

#### Housing Service

**housing.service.ts**

```typescript
import { Injectable } from '@angular/core';
import { HousingLocation } from './housing-location';

@Injectable({
  providedIn: 'root'
})
export class HousingService {
  url = 'http://localhost:3000/locations';

  constructor(){}

  async getAllHousingLocations(): Promise<HousingLocation[]> {
    const data = await fetch(this.url);
    return await data.json() ?? [];
  }

  async getHousingLocationById(id: number): Promise<HousingLocation | undefined> {
    const data = await fetch(`${this.url}/${id}`);
    return await data.json() ?? {};  
  }

  submitApplication(firstName: string, lastName: string, email: string) {
    console.log(`Homes application received: firstName: ${firstName}, lastName: ${lastName}, email: ${email}.`);
  }
}
```

### Models

#### Housing Location Model

**housing-location.ts**

```typescript
export interface HousingLocation {
  id: number;
  city: string;
  state: string;
  photo: string;
  wifi: boolean;
  laundry: boolean;
}
```
**Credit to:** *Angular youtube tutorials!*
### Summary

This home renting listing app provides a simple interface to display and filter rental units based on their location, state, photo, Wi-Fi availability, and laundry facilities. It uses Angular for the frontend and JSON Server for the backend, making it easy to set up and extend.
