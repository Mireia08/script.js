document.addEventListener('DOMContentLoaded', () => {
    const habitsContainer = document.getElementById('habits-container');
    const addHabitBtn = document.getElementById('add-habit-btn');
    const progressChartCtx = document.getElementById('progressChart').getContext('2d');

    let habits = [];

    // Function to add a new habit
    function addHabit() {
        const habitName = prompt('Enter the habit name:');
        if (habitName) {
            const habit = {
                name: habitName,
                days: Array(31).fill(false)
            };
            habits.push(habit);
            renderHabits();
        }
    }

    // Function to render habit grids
    function renderHabits() {
        habitsContainer.innerHTML = '';
        habits.forEach((habit, habitIndex) => {
            const habitGrid = document.createElement('div');
            habitGrid.classList.add('habit-grid');
            habitGrid.innerHTML = `<strong>${habit.name}</strong>`;
            habit.days.forEach((completed, dayIndex) => {
                const dayCell = document.createElement('div');
                dayCell.classList.add('day');
                dayCell.textContent = dayIndex + 1;
                if (completed) {
                    dayCell.classList.add('completed');
                }
                dayCell.addEventListener('click', () => {
                    habit.days[dayIndex] = !habit.days[dayIndex];
                    renderHabits();
                    updateChart();
                });
                habitGrid.appendChild(dayCell);
            });
            habitsContainer.appendChild(habitGrid);
        });
        updateChart();
    }

    // Function to update the progress graph
    function updateChart() {
        const data = habits.map(habit => habit.days.filter(day => day).length);
        const labels = habits.map(habit => habit.name);

        const chartData = {
            labels: labels,
            datasets: [{
                label: 'Completed Days',
                data: data,
                backgroundColor: '#007bff'
            }]
        };

        new Chart(progressChartCtx, {
            type: 'bar',
            data: chartData,
            options: {
                scales: {
                    y: {
                        beginAtZero: true,
                        max: 31
                    }
                }
            }
        });
    }

    addHabitBtn.addEventListener('click', addHabit);

    // Initial render
    renderHabits();
});
