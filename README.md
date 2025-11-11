# Avatar Changer System

## Project Overview
The Avatar Changer System is a Roblox-based module enabling seamless avatar selection and management. It offers a robust server-side avatar handler combined with a sleek client UI for player interaction, featuring avatar validation, cooldowns, and texture preloading.

## Features
- **Server-Side Avatar Management:** Efficient validation, cooldown enforcement, automatic retries, and saving user preferences.
- **Client-Side UI:** Modern, category-based avatar selection interface with smooth animations.
- **Avatar Persistence:** Automatically reapplies avatars on player respawn.
- **Debug Mode:** Detailed logging options for development and troubleshooting.
- **Physically Based Rendering (PBR) Textures:** Enhanced avatar appearance with preloading for performance.

## Getting Started

### Installation
1. Add the server scripts to the `ServerScriptService` in your Roblox project.
2. Insert the client UI script into `StarterPlayer > StarterPlayerScripts`.
3. Ensure `ReplicatedStorage` contains the RemoteEvents: `ChangeAvatar` and `RefreshTitle`.
4. Populate the `AvatarStorage` folder in `ReplicatedStorage` with UserId folders under `Boy` and `Girl`.
5. Optionally enable debug mode in the server script for verbose logging.

### Usage
- Players navigate the client UI to browse and select avatars.
- The server handles avatar application with appropriate validations and cooldowns.
- Avatars persist through player respawns and can be reset or reapplied as needed.

## Configuration
- Adjust cooldown times, retry attempts, and debug flags in server-side scripts.
- Update the avatar user lists in the avatar loader script as needed.

## Contributing
Contributions are welcome! Feel free to fork this repository, propose improvements, and submit pull requests.

## Author
ItoRenz00
