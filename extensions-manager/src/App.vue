<template>
  <div class="header-div">
    <box-component :is-extension="false">
      <span class="header-left">
        <img src="./assets/images/logo-cropped.svg" 
        alt="App Logo" 
        class="app-logo"/>
        <span>Extensions</span>
      </span>
      <toggle-theme-btn/>
    </box-component>
  </div>
  <div class="extensions-header-div"> 
    <div class="extensionsData-header">
      Extensions List
    </div>
    <div class="filter-btns">
      <btn-component btn-function="sort-all" 
        :class="filterBtnArr[FilterBtns.All] ? 'filtering' : ''"
        @sort-all="getAllExtensionsData"/>
      <btn-component btn-function="sort-active"
        :class="filterBtnArr[FilterBtns.Active] ? 'filtering' : ''"
        @sort-active="getActiveExtensionsData"/>
      <btn-component btn-function="sort-inactive"
        :class="filterBtnArr[FilterBtns.Inactive] ? 'filtering' : ''"
        @sort-inactive="getInactiveExtensionsData"/>
    </div>
  </div>
  <div>
    <span class="err-msg">{{ err }}</span>
  </div>
  <div class="extensionsData-list">
    <box-component v-for="extension in display" :key="extension.name">
      <table>
        <tbody>
          <tr class="extension-details">
            <td>
              <img class="extension-img"
              :src="resolveImg(extension)"
              :alt="resolveAlt(extension)"/>
            </td>
            <td>
              <div class="extension-name">
                {{  extension.name }}
              </div>
              <div class="extension-desc">
                {{  extension.desc }}
              </div>
            </td>
          </tr>
          <tr>
            <td colspan="2">
              <div class="functionality-row">
                <div class="function-left">
                  <btn-component
                    @remove-extension="removeExtension(extension)"
                  />
                </div>
                <div class="function-right"><toggle-switch :is-active="extension.isActive" @toggle-active="toggleActive(extension)"/></div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </box-component>
  </div>
</template>

<script>
import BoxComponent from './components/BoxComponent.vue';
import ToggleThemeBtn from './components/ToggleThemeBtn.vue';
import ToggleSwitch from './components/ToggleSwitch.vue';

import jsonData from './assets/data.json';
import BtnComponent from './components/BtnComponent.vue';

export default {
  name: 'App',
  components: {
    BoxComponent,
    ToggleThemeBtn,
    ToggleSwitch, 
    BtnComponent,
  },
  data() {
    return {
      extensionsData: jsonData.extensions || [],
      display: [],
      err: "",
      FilterBtns: ({
        All: 0,
        Active: 1,
        Inactive: 2,
      }),
      filterBtnArr: [true, false, false],
    };
  },
  mounted() {
    try {
      if (this.extensionsData.length == 0 || !this.extensionsData.length)
        throw new Error('No extensionsData data found in JSON file');
      this.display = this.extensionsData;
    } catch (e) {
      this.err = e;
      console.error('Could not load extensionsData data from JSON file', e);
    }
  },
  methods: {
    resolveImg(extension) {
      try {
        return new URL(`./assets/images/${extension.img}`, import.meta.url).href;
      } catch (e) {
        console.error(`Could not resolves extension image for ${extension.name}: ` + e);
        return '';
      }
    },
    resolveAlt(extension) {
      try {
        return extension.alt;
      } catch (e) {
        this.err = e;
        console.error("Could not resolve extension image alt: " + e);
      }
    },
    toggleActive(extension) {
      try {
        // Find the index of the object in the master list
        const idx = this.extensionsData.findIndex(ext => ext.name === extension.name);
        
        if (idx !== -1) {
          this.extensionsData[idx].isActive = !this.extensionsData[idx].isActive;
          this.updateDisplay();
        }
        else
          throw new Error("Extension not found: " + extension.name);
      } catch (e) {
        this.err = e;
        console.error("Could not toggle extension active state: " + e);
      }
    },
    removeExtension(extension) {
      try {
        // Find the index of the object in the master list
        const idx = this.extensionsData.findIndex(ext => ext.name === extension.name);
        
        if (idx !== -1) {
          this.extensionsData.splice(idx, 1);
          this.updateDisplay();
        }
      } catch (e) {
        this.err = e;
        console.error("Could not remove extension: " + e);
      }
    },
    getAllExtensionsData() {
      this.display = this.extensionsData;
      this.resolveBtnHighlight(this.FilterBtns.All);
    },
    getActiveExtensionsData() {
      this.display = this.extensionsData.filter(ext => ext.isActive);
      this.resolveBtnHighlight(this.FilterBtns.Active);
    },
    getInactiveExtensionsData() {
      this.display = this.extensionsData.filter(ext => !ext.isActive);
      this.resolveBtnHighlight(this.FilterBtns.Inactive);
    },
    updateDisplay() {
      for (var i = 0; i < this.filterBtnArr.length; i++) {
        if (this.filterBtnArr[i]) {
          if (i === this.FilterBtns.All) {
            this.getAllExtensionsData();
          } else if (i === this.FilterBtns.Active) {
            this.getActiveExtensionsData();
          } else if (i === this.FilterBtns.Inactive) {
            this.getInactiveExtensionsData();
          }
          break;
        }
      }
    },
    resolveBtnHighlight(idx) {
      this.filterBtnArr = this.filterBtnArr.map((_, i) => i === idx);
    }
  }
};
</script>
<style>
.app-logo {
  height: 30px;
  margin-right: 0.5rem;
}
.header-left {
  display: flex;
  align-items: center;
}
.header-div {
  margin-bottom: 1rem;
}
.extensionsData-header, .extension-name {
  font-weight: bold;
}
.extensionsData-header {
  display: flex;

  justify-content: space-between;
  font-size: larger;
}
.extensions-header-div {
  display: flex;
  justify-content: space-between;
}
.filter-btns {
  margin: 10px 0;
  display: flex;
  gap: 10px;
}
.filtering {
  background-color: var(--medium-accent);
  color: var(--primary);
  border: 2px solid var(--medium-accent);
  &:hover {
    border: 2px solid var(--medium-accent);
    color: var(--primary-font);
    background-color: var(--secondary);
  }
}
@media screen and (max-width: 600px) {
    .extensionsData-header {
      flex-direction: column;
      align-items: center;
    }
    .filter-btns {
      justify-content: center;
    }
}
.extensionsData-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.err-msg {
  color: var(--medium-accent);
}
.extension-details {
  display: flex;
  align-items: start;
}
.extension-img {
  width: 50px;
  padding-right: 10px;
}
.extension-desc {
  color: var(--secondary-font);
}
.functionality-row {
  display: flex;
  justify-content: space-between;
  width: 100%;
  align-items: center;
}
.function-left, .function-right {
  display: flex;
  align-items: center;
}
</style>
