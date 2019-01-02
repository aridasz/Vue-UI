<template>
  <div class="container-fluid">

    <div class="header clearfix">
      <h2><img src="../assets/logo.png" width=35> Document Finder</h2>
    </div>

	<div class="row">
		<div class="col-md-7 col-lg-7">
			<div class="container">
			  <SearchBox v-model="searchTerm" />      
			</div>
			<div style="height:400px; overflow: auto;">
				<TutorialList :tutorials="pageOfTutorials" />
			</div>
			<Pagination v-model="page" :items="tutorials.length" :perPage="10" />
		</div>
		<div class="col-md-5 col-lg-5">
			<iframe id="MyFrame"
			  name="MyFrame"
			  style="height: 500px; border: 2px; width: 100%"
			  src="assets/pdfjs/web/viewer.html?file=http://localhost:8080/assets/v.pdf#search=render"></iframe>
		</div>
	</div>
    

    
	

    <footer class="footer">
      <p></p>
    </footer>

  </div>
</template>

<script>
import TutorialList from './TutorialList'
import Pagination from './Pagination'
import SearchBox from './SearchBox'
import RadioGroup from './RadioGroup'
import getArraySection from '../utilities/get-array-section'
import { tutorials as tutorialData } from '../data'


export default {
  name: 'app',
  components: { TutorialList, Pagination, SearchBox, RadioGroup },
  data: () => ({
    searchTerm: '',
    tech: '',
    tutorials: [],
    page: 1
  }),
  computed: {
    pageOfTutorials: function() {
      return getArraySection(this.tutorials, this.page, 10)
    }
  },
  watch: {
    searchTerm: function() {
      this.filterTutorials()
    },
    tech: function() {
      this.filterTutorials()
    }
  },
  methods: {
    filterTutorials: function() {
      const searchTerm = this.searchTerm.toLowerCase()
      const tech = this.tech
      let result = tutorialData

      if (searchTerm) {
        result = result.filter(tutorial => {
          return (
            tutorial.title.toLowerCase().search(searchTerm) >= 0 ||
            tutorial.description.toLowerCase().search(searchTerm) >= 0
          )
        })
      }

      if (tech) {
        result = result.filter(tutorial => tutorial.tech.indexOf(tech) >= 0)
      }

      this.tutorials = result
      this.page = 1
    }
  },
  created: function() {
    this.filterTutorials()
  }
}
</script>

<style scoped>
  .container {
    max-width: 750px;
  }
  .footer {
    padding: 15px;
    color: #777;
    border-top: 1px solid #e5e5e5;
  }
  .header {
    margin: 10px 0 20px;
  }
</style>