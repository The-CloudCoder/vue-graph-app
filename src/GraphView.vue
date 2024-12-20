<template>
    <div ref="chart" class="graph-container"></div>
  </template>
  
  <script>
  import * as d3 from 'd3'
  
  export default {
    data() {
      return {
        graphData: {
          nodes: [
            { id: '0. Foundations', group: 0 },
            { id: '1. Basic ROS2', group: 1 },
            { id: '2. ROS Basics', group: 2 },
            { id: '3. Intermediate ROS2', group: 3 },
            { id: 'Intermediate ROS2 (Python)', group: 3 },
            { id: 'Intermediate ROS2 (C++)', group: 3 },
            { id: 'Advanced Modern C++ for robotics', group: 3 },
            { id: 'Behavior Trees for ROS2', group: 3 },
            { id: 'ROS1-ROS2 Migration', group: 4 },
            { id: 'ROS2 Basics in 5 days (C++)', group: 4 },
            { id: 'ROS2 Basics in 5 days (Python)', group: 4 },
            { id: 'ROS2 Security', group: 4 },
            { id: 'URDF robot modelling ROS2', group: 4 },
            { id: 'Linux for robotics', group: 4 },
            { id: 'C++ Basics for robotics', group: 4 },
            { id: 'Python 3 for robotics', group: 4 },
            { id: 'TF in ROS2', group: 4 },
            { id: 'Gazebo Sim with ROS2', group: 5 },
            { id: '5. Navigation ROS2', group: 5 },
            { id: 'Nav2 Basics', group: 5 },
            { id: 'Nav Advanced', group: 5 },
            { id: '7. Manipulation', group: 5 },
            { id: '8. Robot Creation', group: 5 },
            { id: 'RTAB', group: 5 },
            { id: 'ROS2 Basics in 3 days (Rust)', group: 5 },
            { id: '9. Artificial Intelligence', group: 6 },
            { id: '10. ROS Debugging', group: 6 },
            { id: '22. Web Devel for Robots', group: 6 },
            { id: 'ROS2 Control', group: 6 },
            { id: '4. Robotics Theory', group: 7 },
            { id: '14. Enterprise', group: 7 },
            { id: '11. Course of product', group: 7 },
            { id: 'create a link', group: 2 },
            { id: '13. Simulation', group: 2 }
          ],
          links: [
            { source: '0. Foundations', target: '1. Basic ROS2' },
            { source: '1. Basic ROS2', target: '2. ROS Basics' },
            { source: '1. Basic ROS2', target: '3. Intermediate ROS2' },
            { source: '3. Intermediate ROS2', target: 'Intermediate ROS2 (Python)' },
            { source: '3. Intermediate ROS2', target: 'Intermediate ROS2 (C++)' },
            { source: '3. Intermediate ROS2', target: 'Advanced Modern C++ for robotics' },
            { source: '3. Intermediate ROS2', target: 'Behavior Trees for ROS2' },
            { source: '3. Intermediate ROS2', target: 'ROS1-ROS2 Migration' },
            { source: '3. Intermediate ROS2', target: 'ROS2 Basics in 5 days (C++)' },
            { source: '3. Intermediate ROS2', target: 'ROS2 Basics in 5 days (Python)' },
            { source: '3. Intermediate ROS2', target: 'ROS2 Control' },
            { source: '3. Intermediate ROS2', target: '10. ROS Debugging' },
            { source: '3. Intermediate ROS2', target: '22. Web Devel for Robots' },
            { source: '1. Basic ROS2', target: 'ROS2 Security' },
            { source: '1. Basic ROS2', target: 'URDF robot modelling ROS2' },
            { source: '0. Foundations', target: 'C++ Basics for robotics' },
            { source: '0. Foundations', target: 'create a link' },
            { source: '2. ROS Basics', target: 'Linux for robotics' },
            { source: '2. ROS Basics', target: 'TF in ROS2' },
            { source: '2. ROS Basics', target: 'Python 3 for robotics' },
            { source: '2. ROS Basics', target: '13. Simulation' },
            { source: '1. Basic ROS2', target: '5. Navigation ROS2' },
            { source: '5. Navigation ROS2', target: 'Nav2 Basics' },
            { source: '5. Navigation ROS2', target: 'Nav Advanced' },
            { source: '5. Navigation ROS2', target: 'ROS2 Basics in 3 days (Rust)' },
            { source: '5. Navigation ROS2', target: 'RTAB' },
            { source: '1. Basic ROS2', target: 'Gazebo Sim with ROS2' },
            { source: '1. Basic ROS2', target: '8. Robot Creation' },
            { source: '1. Basic ROS2', target: '7. Manipulation' },
            { source: '3. Intermediate ROS2', target: '9. Artificial Intelligence' },
            { source: '4. Robotics Theory', target: '14. Enterprise' },
            { source: '4. Robotics Theory', target: '11. Course of product' }
          ]
        }
      }
    },
    mounted() {
      this.createGraph()
    },
    methods: {
      createGraph() {
        const width = 800
        const height = 600
  
        const highlightColor = 'orange'
        const selectedNodeOpacity = 1.0
        const connectedOpacity = 0.7
        const dimOpacity = 0.2
  
        const svg = d3.select(this.$refs.chart)
          .append('svg')
          .attr('width', width)
          .attr('height', height)
  
        const simulation = d3.forceSimulation(this.graphData.nodes)
          .force('link', d3.forceLink(this.graphData.links).id(d => d.id).distance(100))
          .force('charge', d3.forceManyBody().strength(-300))
          .force('center', d3.forceCenter(width / 2, height / 2))
          .force('collide', d3.forceCollide().radius(15))
  
        const link = svg.append('g')
          .attr('class', 'links')
          .attr('stroke', '#999')
          .attr('stroke-opacity', 0.6)
          .selectAll('line')
          .data(this.graphData.links)
          .enter().append('line')
          .attr('stroke-width', 1)
  
        const node = svg.append('g')
          .attr('class', 'nodes')
          .attr('stroke', '#fff')
          .attr('stroke-width', 1.5)
          .selectAll('circle')
          .data(this.graphData.nodes)
          .enter().append('circle')
          .attr('r', 10)
          .attr('fill', '#555')
          .call(d3.drag()
            .on('start', (event, d) => {
              if (!event.active) simulation.alphaTarget(0.3).restart()
              d.fx = d.x
              d.fy = d.y
            })
            .on('drag', (event, d) => {
              d.fx = event.x
              d.fy = event.y
            })
            .on('end', (event, d) => {
              if (!event.active) simulation.alphaTarget(0)
              d.fx = null
              d.fy = null
            })
          )
          .on('click', (event, d) => {
            // On selection, highlight the selected node and its "tree"
            // First, determine which nodes and links are connected to the selected node
            const connectedNodes = new Set([d.id])
            const connectedLinks = []
            
            // Find links that are connected to d
            this.graphData.links.forEach(l => {
              if (l.source.id === d.id) {
                connectedNodes.add(l.target.id)
                connectedLinks.push(l)
              } else if (l.target.id === d.id) {
                connectedNodes.add(l.source.id)
                connectedLinks.push(l)
              }
            })
  
            // Dim all nodes and links
            node.attr('fill', '#555').attr('opacity', dimOpacity)
            link.attr('stroke', '#999').attr('stroke-opacity', dimOpacity)
  
            // Highlight selected node
            d3.select(event.currentTarget)
              .attr('fill', highlightColor)
              .attr('opacity', selectedNodeOpacity)
  
            // Highlight connected nodes
            node.filter(nd => connectedNodes.has(nd.id) && nd.id !== d.id)
              .attr('fill', highlightColor)
              .attr('opacity', connectedOpacity)
  
            // Highlight connected links
            link.filter(lk => connectedLinks.includes(lk))
              .attr('stroke', highlightColor)
              .attr('stroke-opacity', connectedOpacity)
          })
  
        const labels = svg.append('g')
          .attr('class', 'labels')
          .selectAll('text')
          .data(this.graphData.nodes)
          .enter().append('text')
          .text(d => d.id)
          .attr('font-size', '10px')
          .attr('dx', 12)
          .attr('dy', '.35em')
  
        simulation.on('tick', () => {
          link
            .attr('x1', d => d.source.x)
            .attr('y1', d => d.source.y)
            .attr('x2', d => d.target.x)
            .attr('y2', d => d.target.y)
  
          node
            .attr('cx', d => d.x)
            .attr('cy', d => d.y)
  
          labels
            .attr('x', d => d.x)
            .attr('y', d => d.y)
        })
  
        // Zoom and Pan
        const zoom = d3.zoom()
          .scaleExtent([0.5, 5])
          .on('zoom', (event) => {
            svg.selectAll('g')
              .attr('transform', event.transform)
          })
  
        svg.call(zoom)
      }
    }
  }
  </script>
  
  <style scoped>
  .graph-container {
    border: 1px solid #ccc;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 20px auto;
    max-width: 1000px;
  }
  </style>
  