#!/usr/bin/env node

/*
       Licensed to the Apache Software Foundation (ASF) under one
       or more contributor license agreements.  See the NOTICE file
       distributed with this work for additional information
       regarding copyright ownership.  The ASF licenses this file
       to you under the Apache License, Version 2.0 (the
       "License"); you may not use this file except in compliance
       with the License.  You may obtain a copy of the License at

         http://www.apache.org/licenses/LICENSE-2.0

       Unless required by applicable law or agreed to in writing,
       software distributed under the License is distributed on an
       "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
       KIND, either express or implied.  See the License for the
       specific language governing permissions and limitations
       under the License.
*/

var shell   = require('shelljs'),
    path    = require('path'),
    fs      = require('fs'),
    ROOT    = path.join(__dirname, '..');

var www_dir = path.join(__dirname,"..","..","www");

var project_dir=path.join(__dirname,"..","project");


var project_template_dir=path.join(__dirname,"..","..","project_template")

var sugar_cordova_dir=path.join(__dirname,"..");

function copy_files_to_prj_dir()
{



	fs.readdir(project_template_dir,function(err,files){
		if (err) console.log(err);
		else
		{
		    var i=0;
		    for(i=0;i<files.length;i++)
		    {
			var file=files[i];
			file_path = path.join(project_template_dir+"/"+file);
			stats = fs.lstatSync(file_path);
			if (!stats.isDirectory()) {
				shell.cp('-f',file_path, project_dir);
			}

			if (stats.isDirectory()) {
				shell.cp('-rf', file_path, project_dir);
			}	
	
		    }
		}

	});



	var platform_www=path.join(__dirname,"..","..","platform_www");

	fs.readdir(platform_www,function(err,files){
		if (err) console.log(err);
		else
		{
		    var i=0;
		    for(i=0;i<files.length;i++)
		    {
			
			var file=files[i];
			file_path = path.join(platform_www+"/"+file);
			stats = fs.lstatSync(file_path);
			if (!stats.isDirectory()) {
				shell.cp('-f',file_path, project_dir);
			}

			if (stats.isDirectory()) {
				shell.cp('-rf', file_path, project_dir);
			}
		    }
		}

	});



	fs.readdir(www_dir,function(err,files){
		if (err) console.log(err);
		else
		{
		    var i=0;
		    for(i=0;i<files.length;i++)
		    {
			
			var file=files[i];
			file_path = path.join(www_dir+"/"+file);
			stats = fs.lstatSync(file_path);
			if (!stats.isDirectory()) {
				shell.cp('-f',file_path, project_dir);
			}

			if (stats.isDirectory()) {
				shell.cp('-rf', file_path, project_dir);
			}			

		    }
		}

	});



}



function generate_xo()
{

	var file_prj_name= path.join(sugar_cordova_dir,'Project_Name.txt');

	fs.readFile(file_prj_name, 'utf8', function (err,data) {
	  if (err) {
	    return console.log(err);
	  }

          var activity_dir=path.join(sugar_cordova_dir,data+".activity");
	  shell.mkdir('-p',activity_dir);
          shell.cp('-rf', path.join(project_dir,'*'),path.join(activity_dir));
	  shell.cd(sugar_cordova_dir);
          zipCommand=process.env.ZIPCOMMAND;
          console.log("zip command : "+zipCommand);
          if(zipCommand == undefined)
          {
	      shell.exec("zip -r "+data+".xo "+data+".activity");
          }
          else
          {
              shell.exec(zipCommand+" "+data+".xo "+data+".activity");
          }
	  console.log("the .xo file is stored at : "+ sugar_cordova_dir);
	});

}


function update_index_html_with_iframe()
{
        shell.rm('-f',path.join(project_dir,'index.html'));
        shell.rm('-f',path.join(project_dir,"users_index_page.html"));

	var prj_template_index_html=path.join(project_template_dir,'index.html');
	var www_index_html=path.join(www_dir,'index.html');

        shell.cp('-f',www_index_html,path.join(project_dir,"users_index_page.html"));


        /*
	fs.readFile(www_index_html, 'utf8', function (err,data) {
	  if (err) {
	    return console.log(err);
	  }
	  //console.log("index.html : "+data);	  
	  shell.echo(data).to(path.join(project_dir,"users_index_page.html"));

	});
        */


		fs.readFile(prj_template_index_html, 'utf8', function (err,data) {
		  if (err) {
		    return console.log(err);
		  }
		  //console.log("index.html : "+data);	  

	

	var body_before_canvas = data.match(/<body[^>]*>([\s\S]*?)<div id=\"canvas\">/g);
	//console.log("html content from body tag till canvas div"+body_before_canvas);
	updated_body_content=body_before_canvas[0].concat("\n  <iframe  src=\"users_index_page.html\" style=\"width:100%;height:100%\" scrolling=\"no\"></iframe>\n</div>\n</body>");
	updated_data=data.replace(/<body[^>]*>([\s\S]*?)<\/body>/g,updated_body_content);


			fs.writeFile(path.join(project_dir,'index.html'),updated_data, function(err) {
			    if(err) {
				console.log(err);
			    } else {
				console.log("updated index.html");
				generate_xo();
			    }
			});		  		  

		  
		});


}

function default_update_index_html()
{

shell.rm('-f',path.join(project_dir,'index.html'));
shell.cp('-f',path.join(www_dir,'index.html'),path.join(project_dir,'index.html'));
generate_xo();

}


module.exports.run = function(option) {

shell.rm('-rf',project_dir);

shell.mkdir('-p',project_dir);

copy_files_to_prj_dir();

option = typeof option !== 'undefined' ? option : "hello";

//update index.html according to the option mentioned
if(option == 'noiframe')
{
  default_update_index_html()
}
else
{
  update_index_html_with_iframe()
}

}


module.exports.help = function() {
    console.log('Give the help instructions for sugar here');
    process.exit(0);
}
