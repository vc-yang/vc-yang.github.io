---
layout: page
title: Visualizing the US congress
permalink: /vis_congress/
---

<!--
# Visualizing the US congress
-->
Published Nov 2016

Last updated March 2018

This is a interactive visualization I made of the congress members' ideology positions, reduced to 2 dimensions, using the [DW-NOMINATE method](https://en.wikipedia.org/wiki/NOMINATE_(scaling_method)). This visualiztaion is developed as part of the IDEAS Focus Summer School on Data Visualization at Northwestern University.

### How to use this visulaization
* Each circle represents a congress member
* The colors represent party membership
* Hover mouse over a circle to see name and state of the congress member. 
* Use the slider bar to scroll over years


<html>
  <head>
    <meta charset="utf-8">
    <title>
        Visualizing the US congress </title>
    <link rel="stylesheet" type="text/css" href="../files/vis_congress/style.css" />
    <script src="https://d3js.org/d3.v3.min.js"></script>
  </head>
  <body>
      

<h4 id = "currentCongress"> congress # 113 </h4>
<h4 id = "congressYear"> year 2013 - 2015</h4>

    <p id="chart_area">
    
    <label for="congressSlider" 
    style="display: inline-block; width: 240px; text-align: right">
    Slide to change year  <span id="congressSlider-value"></span>
  </label>
    <input type = "range" id = "congressSlider" min = "1" max = "113" value= "113">
 
    </p>

      
      <!-- add legend as an image -->
      <img src="../files/vis_congress/legend2.jpg" style="width:556px;height:52px;">

      
    <script>
        
        
    d3.csv('../files/vis_congress/congressData2.csv', function(sheet){
            n = sheet;
        
            congressIdx = 113;

            d3.select("#congressSlider").on("input", function() {
                d3.selectAll("circle").remove()

                congressIdx = parseInt(this.value);
                d3.select("#currentCongress").text("congress # " + this.value);
                d3.select("#congressYear").text("year "+ (1789+(this.value-1)*2) + " - " + (1789+(this.value-1)*2 +2))
                console.log(congressIdx)
                update();
            })  
        
        
            var chart = d3.select("#chart_area")
            var frame = chart.append("svg") // create svg canvas
            
            var canvas = frame.append('g');
            
            var x = 40
            var y = 20
            
            var width = 700;
            var height = 500;
            
            frame.attr('width', width + x); 
            frame.attr('height', height + y*2);
            
            var xScale = d3.scale.linear(); // can search online for this...
            xScale.domain([-1.2, 1.2]); // numbers that are already known
            xScale.range([0, width]); // physical appearance
            
            
            var yScale = d3.scale.linear();
            yScale.domain([-1.2, 1.2]);
            yScale.range([height, 0]);
                  
            var xAxis = d3.svg.axis().orient("bottom").scale(xScale);
            var yAxis = d3.svg.axis().orient("left").scale(yScale);                  
            canvas.append("g").call(xAxis).attr("class", "axis")
            .attr("transform", "translate (0," + height +")")
            // append group"
            canvas.append("g").call(yAxis).attr("class", "axis")  // append group                  
                  
            canvas.attr("transform", "translate(" + x + "," + y +  ")")
            
            // add x label
            canvas.append("text")
        .attr("class", "x label")
        .attr("text-anchor", "end")
        .attr("x", width)
        .attr("y", height - 6)
        //.attr("fontsize",  20)
        .text("DW-NOMINATE 1st dimension (position on government intervention in economy)");
    
        // add y label
        canvas.append("text")
    .attr("class", "y label")
    .attr("text-anchor", "end")
    .attr("y", 6)
    .attr("dy", ".75em")
    .attr("transform", "rotate(-90)")
    .text("DW-NOMINATE 2nd dimension (position on cross-cutting, salient issues of the day)");
            

            function update(){
                var dot = canvas.selectAll("circle")

                var tooltip = d3.select("body")
                    .append("div")
                    .style("position", "absolute")
                    .style("z-index", "10")
                    .style("visibility", "visible")
                
                
                dot.data(sheet.filter(function(d) {
                    return d["congressNumber"] == congressIdx 
                    })) // get data
             
                        .enter()
                
                        .append("circle").attr("class", "dot")
            
                        .style('fill-opacity', 0.5)
                
                        .attr("cx", function(d){return xScale(d["1stDimension"]);})
                        .attr("cy", function(d){return yScale(d['2ndDimension']);})
                        .attr("r", 5)   
                       
                        .style("fill", function(d){
                        if (d["partyCode"] == 100){
                            return "blue"
                        }
                            else if (d["partyCode"] == 200){
                            return "red"
                            }
                        else if (d["partyCode"] == 1){
                            return "green"
                        }
                        else if (d["partyCode"] == 5000){
                            return "teal"
                        }
                     else if (d["partyCode"] == 29){
                            return "magenta"
                        }
                    
                    else if (d["partyCode"] == 555){
                            return "yellow"
                        }
                else if (d["partyCode"] == 1275){
                            return "brown"
                        }
                else if (d["partyCode"] == 3333){
                            return "orange"
                        }
                 else if (d["partyCode"] == 7777){
                            return "purple"
                        }    
                    
                    
                    
                        else {return 'black'}
                        }) 
                
                // 100 - democrat 200- repub
                // 1 - fedralist, 5000  Pro-Administration
                // 29 - wig 555- jackson 1275  Anti-Jackson
                // 3333 -Opposition 7777  Crawford Republican
                  .on("mouseover", function(d){ 
                    tooltip.text(d["name"] +", " + d["stateName"]);
                    tooltip.style("visibility", "visible");
                    tooltip.style("background",'#F4F6F7');
                })
                
                                    
                    .on("mousemove", function(){return tooltip.style("top",(d3.event.pageY-10)+"px").style("left",(d3.event.pageX+10)+"px");})
                    .on("mouseout", function(){return tooltip.style("visibility", "hidden");});
                
                
                
     
                
                
            };
                    
                    
            update();
            
                        
            //console.log(sheet["1stDimension"]);
        
            
    }) 
    </script>
  </body>
</html>

### What are the axes? 
According to Keith Poole and Howard Rosenthal, the creator of the DW-NOMINATE method, the 1st dimension score (horizontal axis) can be interpreted as the position on government intervention in the economy, also referred to as the 'liberal-conservative' scale. The 2nd dimension (vertical axis) can be interpreted as the position on salient issues of the day, such as slavery, civil rights, and LGBT rights. The 1st dimension explains the vast majority of differences in voting behavior. More discussion [here](https://legacy.voteview.com/dwnomin.htm).

Please note that the DW-NOMINATE score is in nature a relative measure that does not reflect into specific policy dimensions. The axes are similar to the principal component axes. Quantifying Congress members' positions on particular policy issues is still a challenge to be tackled. 


### How are the positions calculated? 
The DW-NOMINATE method gives Congress member's relative ideological positions through analyzing how often Congress members vote with or against each other in the roll call records. The DW-NOMINATE method is a scaling method -- it puts those with more similar voting behavior closer together, and vice versa. [Its Wikipedia page](https://en.wikipedia.org/wiki/NOMINATE_(scaling_method)) gives a good explanation of the method. 

### More information
The U.S. Congress members’ roll call vote records are openly available. You can explore the data set and download it from [govtrack.us](https://www.govtrack.us/). The data used in this visualization is hosted on the voteview website [here](https://legacy.voteview.com/dwnomin_joint_house_and_senate.htm). You can find my copy of the csv file, which is the input for this visualization [here](https://github.com/vc-yang/vc-yang.github.io/blob/master/files/vis_congress/congressData2.csv). This interactive visualization is developed using [D3](https://d3js.org/). If you are interested in making similar visualizations, you are welcome to download the source code of this page on [my Github](https://github.com/vc-yang/vc-yang.github.io/tree/master/_pages/0301_vis_congress).

