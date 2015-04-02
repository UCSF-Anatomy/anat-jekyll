---
layout: page
title: allan
permalink: /allan/
---
<html>

    <!-- load a recent version of jQuery -->
    <script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>

<!-- get data from the UCSF Profiles API -->
<script> 
// we're grabbing data from the UCSF Profiles JSON API v2
// details at http://opendata.profiles.ucsf.edu/json-v2.html

add_profiles_user_content('ProfilesURLName', 'allan.basbaum');


function add_profiles_user_content (identifier_type, identifier) {
    $.getJSON('http://api.profiles.ucsf.edu/json/v2/?source=JSON_API_v2_department of anatomy&' + identifier_type + '=' + identifier + '&publications=full&callback=?',
	      function(response) {
		  if (response) {
		      if (response.error) { // if UCSF Profiles reports an error, can do something with that here
			  if (window.console && window.console.log) {
			      console.log('Error from UCSF Profiles API: ' + response.error);
			      // alert('Error from UCSF Profiles API: ' + response.error);
			  }
		      } else {

			  $(document).ready(function() {   // execute the following code after DOM is ready

			      var data = response.Profiles[0];

			      if (data.Name) { // show name, if we have it
				  $('#profiles_name').text(data.Name);
			      }
			      if (data.ProfilesURL) { // add link to Profiles
				  $('#profiles_page_link_container').empty().show().append('<a href="' + data.ProfilesURL + '" title="Go to UCSF Profiles, powered by CTSI" rel="me">UCSF Profiles page</a>');
			      } else {
				  $('#profiles_page_link_container').empty().hide();
			      }
			      if (data.PhotoURL) { // show photo, if we have it
				  $('#profiles_photo_container').empty().show().append($('<img class="img-circle" id="profiles_photo" />').attr('src', data.PhotoURL).error(function(){this.style.display='none'}));
			      } else {
				  $('#profiles_photo_container').empty().hide();
			      }
			      if (data.Narrative) { // show narrative, if we have it
				  // truncate text to 500 characters, and delete any partial last sentence
			          var truncated_narrative = data.Narrative.substr(0,500).replace(/[\s\r\n]/g, " ").replace(/^(.+\.[\s\n\r]).*?$/g, "$1");
				  $('#profiles_narrative').show().text(truncated_narrative);
			      } else {
				  $('#profiles_narrative').hide();
			      }
			      if (data.Email) { // show narrative, if we have it
				  $('#profiles_email_link_container').empty().show().append('<a id="href="mailto:' + data.Email + '">' + data.Email + '</a>');
			      } else {
				  $('#profiles_email_link_container').hide();
			      }
			      if (data.Publications && data.Publications.length > 0) {
				  $('#profiles_publications ol').remove();
				  $('#profiles_publications').show().append($('<ol id="profiles_publications_list"></ol>'));
				  jQuery.each(data.Publications, function() {
				      var li = $('<li id="profiles_publications_li" />'); // create new list item
				      li.append(this.PublicationTitle + ' '); // add the publication name
				      if (this.PublicationSource && this.PublicationSource.length > 0) { // ...and a link, if available
					  li.append($('<a class="profiles_publication_link" />').attr('href', this.PublicationSource[0].PublicationSourceURL).text('View on ' + this.PublicationSource[0].PublicationSourceName));
				      }
				      $('#profiles_publications_list').append(li); // insert the list item
				  });
			      } else {
				  $('#profiles_publications').hide();
			      }
			      if (data.AwardOrHonors && data.AwardOrHonors.length > 0) {
				  $('#profiles_awards_list').remove();
				  $('#profiles_awards').show().append($('<ul id="profiles_awards_list"></ul>'));
				  jQuery.each(data.AwardOrHonors, function() {
				      var li = $('<li />').append(this.Summary);
				      $('#profiles_awards_list').append(li);
				  });
			      } else {
				  $('#profiles_awards').hide();
			      }
			      
			      $("html, body").animate({ scrollTop: 0 }, 100);
			      $("[id^='profiles_']:visible").fadeIn(100).fadeOut(100).fadeIn(100);
			  });

		      }
		  }
	      }
	     );
}

function highlight_profiles_content (seconds) {
    var ms = (seconds || 1) * 1000;
    $( "[id^='profiles_']" ).addClass('highlighted');
    setTimeout(function() {
	$( "[id^='profiles_']" ).removeClass('highlighted');
    }, ms);
}

</script>
  </head>
  <body>
    
    <div class="container">

      <!-- start of content automatically grabbed from Profiles -->
      <div class="jumbotron">
	<div id="profiles_photo_container"></div>
	<h1 id="profiles_name"></h1>
	<p id="profiles_narrative"></p>
      </div>
      <div class="row">
	<div class="col-md-7 col-md-offset-1"> <!-- start first column -->

	  <h2>Connect</h2>
	  <ul>
	    <li id="profiles_email_link_container"></li>
	    <li id="profiles_page_link_container"></li>
	  </ul>

	  <div id="profiles_awards"><h2>Awards</h2></div>
	  <div id="profiles_publications"><h2>Publications</h2></div>


	</div> <!-- /first column -->

    </div> <!-- /container -->