# joylnk.io

joylnk is a link sharing platform empowering creators to easily share and maintain a single list of their links across their social media profiles. This tool centralizes and simplifies the process of curating links for your follows. Visual appeal is important to the success of the project, therefore the product has the built-in ability to select, and customize theme templates that gives your page custom look and feel.

## Architectural Style
The project will be top-level partitioned with a domain driven model (instead of a technically driven one). This is top-level model (aka not a DDD project).

We will use Clean Architecture to decouple the domain use cases from any required infrastructure and third party services, ui, databases, etc. This will also allow us to easily test the application and separate business logic from implementation details.

In this document we are identifying system components, their modules, the API of the modules and the interfaces required to create cross module communication. 

## Identifying Components
We have identified some top-level components by grouping assigning user stories to appropriate components. We've used the actor/actions method described in [[Component Based Thinking]]

### Users
- As a system, I want to be able to confirm a user's ownership of an email address via confirmation email

#### Creators
- As a creator I want to be able to signin with TikTok or creatorname / email
- As a creator I want to be able to register a custom creatorname that is my account landing page
- As a creator I want to be able to reset my password if i forget it.

#### Visitors
- As a visitor I want to be able to browse creator’s link pages and click on links
- As a visitor I want to be able to subscribe to creators and get email notified when that creator posts a new link

### Theme
- As a creator I want to be able to select a prebuilt theme for my link page.
- As a creator I want to be able to customize my theme, having started with a blank template or a prebuilt template, i want ot customize some or all of the theme
- As a creator I want to be able to add a custom background image to my link page
- As a creator I want to be able to customize the background color with a solid color or gradient.

### Links
- As a creator I want to be able to create, update, read and delete all of my links
- As a creator, I want to be able to shorten my link URLs with or without a custom post-fix

### Analytics
- As a creator I want to be able to analyze how many clicks my links have received in meaningful date ranges (today, last 7 days, last month, all time)
- As a creator i want to see where people are clicking my links from

### Actor/Actions Approach
We have identified actors, actions and then grouped actions into components. 
##### Users (Component)
- Confirm user's ownership of an email address via confirmation email
- Signin with TikTok or creatorname / email
- Register a custom creator name that is my account landing page endpoint
- Reset my password if I forget it
- Subscribe to creators and get email notified when that creator posts a new link
- Unsubscribe to creator updates
- Ensure usernames are available
- Send notifications to users

##### Theme (Component)
- Select a prebuilt theme for my link page
- Customize my theme, having started with a blank template or a prebuilt template, I want to customize some or all of the theme
- Add a custom background image to my link page
- Customize the background color with a solid color or gradient

##### Links (Component)
- Create, update, read, and delete all of my links
- Shorten my link URLs with or without a custom post-fix
- Visit creator’s link pages
- Click on links
- Shorten Links

##### Analytics (Component)
- Analyze how many clicks my links have received in meaningful date ranges (today, last 7 days, last month, all time)
- See where people are clicking my links from
- Track link clicks

#### Define Interfaces
Below we will identify the required interfaces that expose the actions of each component. These are the means by which actors interact with the components.

##### Users Interface:

```typescript
interface UserService {
  confirmEmail(userId: string): Promise<boolean>;
  signInWith(provider: string): Promise<User | null>;
  registerCreatorName(name: string): Promise<boolean>;
  resetUserPassword(userId: string): Promise<boolean>;
  subscribeToCreator(creatorId: string, userId: string): Promise<boolean>;
  unsubscribeFromCreator(creatorId: string, userId: string): Promise<boolean>;
  checkUsernameAvailability(username: string): Promise<boolean>;
  sendNotification(userId: string, notification: Notification): Promise<boolean>;
}
```

##### Theme Interface:

```typescript
interface ThemeService {
  selectPrebuiltTheme(themeId: string, userId: string): Promise<boolean>;
  customizeTheme(themeId: string, userId: string, customizationOptions: ThemeCustomizationOptions): Promise<boolean>;
  addBackgroundImage(userId: string, image: Image): Promise<boolean>;
  customizeBackgroundColor(userId: string, color: Color | Gradient): Promise<boolean>;
}
```

##### Links Interface:

```typescript
interface LinkService {
  createLink(userId: string, linkData: Partial<Link>): Promise<Link>;
  getLink(linkId: string): Promise<Link>;
  updateLink(userId: string, linkId: string, linkData: Partial<Link>): Promise<Link>;
  deleteLink(userId: string, linkId: string): Promise<boolean>;
  shortenLink(link: Link, postfix?: string): Promise<string>;
  visitLinkPage(creatorId: string): Promise<LinkPage>;
  clickLink(linkId: string): Promise<boolean>;
}
}```

##### Analytics Interface:

```typescript
interface AnalyticsService {
  analyzeLinkClicks(userId: string, dateRange: 'today' | 'last 7 days' | 'last month' | 'all time'): Promise<LinkClickAnalysis>;
  getLinkClickSources(userId: string): Promise<LinkClickSource[]>;
  trackLinkClick(linkId: string, userId: string): Promise<boolean>;
}```
