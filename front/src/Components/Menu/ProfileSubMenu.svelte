<script lang="typescript">
    import {gameManager} from "../../Phaser/Game/GameManager";
    import {SelectCompanionScene, SelectCompanionSceneName} from "../../Phaser/Login/SelectCompanionScene";
    import {menuIconVisiblilityStore, menuVisiblilityStore, userIsConnected} from "../../Stores/MenuStore";
    import {selectCompanionSceneVisibleStore} from "../../Stores/SelectCompanionStore";
    import {LoginScene, LoginSceneName} from "../../Phaser/Login/LoginScene";
    import {loginSceneVisibleStore} from "../../Stores/LoginSceneStore";
    import {selectCharacterSceneVisibleStore} from "../../Stores/SelectCharacterStore";
    import {SelectCharacterScene, SelectCharacterSceneName} from "../../Phaser/Login/SelectCharacterScene";
    import {connectionManager} from "../../Connexion/ConnectionManager";
    import {PROFILE_URL} from "../../Enum/EnvironmentVariable";
    import {localUserStore} from "../../Connexion/LocalUserStore";
    import {EnableCameraScene, EnableCameraSceneName} from "../../Phaser/Login/EnableCameraScene";
    import {enableCameraSceneVisibilityStore} from "../../Stores/MediaStore";
    import btnProfileSubMenuCamera from "../images/btn-menu-profile-camera.svg";
    import btnProfileSubMenuIdentity from "../images/btn-menu-profile-identity.svg";
    import btnProfileSubMenuCompanion from "../images/btn-menu-profile-companion.svg";
    import btnProfileSubMenuWoka from "../images/btn-menu-profile-woka.svg";


    function disableMenuStores(){
        menuVisiblilityStore.set(false);
        menuIconVisiblilityStore.set(false);
    }

    function openEditCompanionScene(){
        disableMenuStores();
        selectCompanionSceneVisibleStore.set(true);
        gameManager.leaveGame(SelectCompanionSceneName,new SelectCompanionScene());
    }

    function openEditNameScene(){
        disableMenuStores();
        loginSceneVisibleStore.set(true);
        gameManager.leaveGame(LoginSceneName,new LoginScene());
    }

    function openEditSkinScene(){
        disableMenuStores();
        selectCharacterSceneVisibleStore.set(true);
        gameManager.leaveGame(SelectCharacterSceneName,new SelectCharacterScene());
    }

    function logOut(){
        disableMenuStores();
        loginSceneVisibleStore.set(true);
        connectionManager.logout();
    }

    function getProfileUrl(){
        return PROFILE_URL + `?token=${localUserStore.getAuthToken()}`;
    }

    function openEnableCameraScene(){
        disableMenuStores();
        enableCameraSceneVisibilityStore.showEnableCameraScene();
        gameManager.leaveGame(EnableCameraSceneName,new EnableCameraScene());
    }
</script>

<div class="customize-main">
    <div class="submenu">
        <section>
            <button type="button" class="nes-btn" on:click|preventDefault={openEditNameScene}>
                <img src={btnProfileSubMenuIdentity} alt="Edit your name">
                <span class="btn-hover">Edit your name</span>
            </button>
            <button type="button" class="nes-btn" on:click|preventDefault={openEditSkinScene}>
                <img src={btnProfileSubMenuWoka} alt="Edit your WOKA">
                <span class="btn-hover">Edit your WOKA</span>
            </button>
            <button type="button" class="nes-btn" on:click|preventDefault={openEditCompanionScene}>
                <img src={btnProfileSubMenuCompanion} alt="Edit your companion">
                <span class="btn-hover">Edit your companion</span>
            </button>
            <button type="button" class="nes-btn" on:click|preventDefault={openEnableCameraScene}>
                <img src={btnProfileSubMenuCamera} alt="Edit your camera">
                <span class="btn-hover">Edit your camera</span>
            </button>
        </section>
    </div>

    <div class="content">
        {#if $userIsConnected}
            <section>
                {#if PROFILE_URL != undefined}
                    <iframe title="profile" src="{getProfileUrl()}"></iframe>
                {/if}
            </section>
            <section>
                <button type="button" class="nes-btn" on:click|preventDefault={logOut}>Log out</button>
            </section>
        {:else}
            <section>
                <a type="button" class="nes-btn" href="/login">Sign in</a>
            </section>
        {/if}
    </div>
</div>

<style lang="scss">
    div.customize-main{
      width: 100%;
      display: inline-flex;

      div.submenu{
        height: 100%;
        width: 50px;

        button {
          transition: all .5s ease;
          text-align: left;
          white-space: nowrap;
          margin-bottom: 10px;
          max-height: 44px;

          img {
            height: 26px;
            width: 26px;
            cursor: pointer;
          }

          span.btn-hover{
            display: none;
            font-family: "Press Start 2P";
          }

          &:hover{
            width: auto;

            span.btn-hover {
              display: initial;
            }
          }
        }
      }

      div.content {
        width: 100%;
        section {
          display: flex;
          justify-content: center;
          align-items: center;
          flex-wrap: wrap;
          margin-bottom: 20px;

          iframe {
            width: 100%;
            height: 50vh;
            border: none;
          }

          button {
            height: 50px;
            width: 250px;
          }
        }
      }
    }

    @media only screen and (max-width: 800px) {
      div.customize-main.content section button {
        width: 130px;
      }
    }
</style>