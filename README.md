# incident-trend-analysis
Trend report dashboard
    function generateDashboard(data) {
        const incidentTypes = [...new Set(data.map(d => d.INCIDENT))];
        const incidentCounts = incidentTypes.map(type => data.filter(d => d.INCIDENT === type).length);

        Plotly.newPlot('incidentFrequency', [{
            x: incidentTypes,
            y: incidentCounts,
            type: 'bar'
        }], { title: 'Incident Frequency by Type' });

        const dates = [...new Set(data.map(d => d.DATE))];
        const severities = ['LOW', 'MEDIUM', 'HIGH'];
        const heatmapData = severities.map(severity =>
            dates.map(date => data.filter(d => d.DATE === date && d.SEVERITY === severity).length)
        );

        Plotly.newPlot('severityHeatmap', [{
            z: heatmapData,
            x: dates,
            y: severities,
            type: 'heatmap',
            colorscale: 'Viridis'
        }], { title: 'Severity Trends Over Time (Heatmap)' });

        const workplaces = [...new Set(data.map(d => d.WORKPLACE))];
        const workplaceCounts = workplaces.map(place => data.filter(d => d.WORKPLACE === place).length);

        Plotly.newPlot('workplaceDistribution', [{
            x: workplaces,
            y: workplaceCounts,
            type: 'bar'
        }], { title: 'Workplace Incident Distribution' });

        const actionTimes = [...new Set(data.map(d => d['ACTION TIME']))];
        const actionTimeCounts = actionTimes.map(time => data.filter(d => d['ACTION TIME'] === time).length);

        Plotly.newPlot('actionTimeTrend', [{
            x: actionTimes,
            y: actionTimeCounts,
            type: 'scatter',
            mode: 'lines+markers'
        }], { title: 'Action Time Analysis' });

        const injuredCount = data.filter(d => d.INJURED === 'YES').length;
        const notInjuredCount = data.filter(d => d['NOT-INJURED'] === 'YES').length;

        Plotly.newPlot('injuredVsNotInjured', [{
            labels: ['Injured', 'Not Injured'],
            values: [injuredCount, notInjuredCount],
            type: 'pie'
        }], { title: 'Injured vs Not-Injured Incidents' });

        const times = [...new Set(data.map(d => d.TIME))];
        const timeCounts = times.map(time => data.filter(d => d.TIME === time).length);

        Plotly.newPlot('incidentTimingTrend', [{
            x: times,
            y: timeCounts,
            type: 'scatter',
            mode: 'lines+markers'
        }], { title: 'Incident Timing Trends' });
    }
</script>
