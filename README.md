<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grade Calculator</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --primary: #4f46e5;
            --secondary: #f9fafb;
            --accent: #10b981;
            --danger: #ef4444;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        
        .grade-card {
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        
        .grade-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        
        .grade-A {
            background-color: #d1fae5;
            color: #065f46;
        }
        
        .grade-B {
            background-color: #dbeafe;
            color: #1e40af;
        }
        
        .grade-C {
            background-color: #fef3c7;
            color: #92400e;
        }
        
        .grade-D {
            background-color: #fee2e2;
            color: #991b1b;
        }
        
        .grade-F {
            background-color: #fecaca;
            color: #7f1d1d;
        }
        
        .input-field:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(79, 70, 229, 0.2);
        }
        
        .add-btn {
            transition: all 0.2s ease;
        }
        
        .add-btn:hover {
            transform: scale(1.05);
        }
        
        .result-animation {
            animation: fadeIn 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="min-h-screen py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-3xl mx-auto">
        <div class="text-center mb-10">
            <h1 class="text-3xl font-bold text-gray-900 sm:text-4xl mb-3">Grade Calculator</h1>
            <p class="text-lg text-gray-600">Calculate your final grade based on assignments, exams, and other components</p>
        </div>
        
        <div class="bg-white rounded-xl grade-card p-6 mb-8">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-2 sm:mb-0">Add Grade Components</h2>
                <button id="resetBtn" class="px-4 py-2 bg-gray-100 text-gray-700 rounded-lg hover:bg-gray-200 transition">
                    <i class="fas fa-redo mr-2"></i>Reset All
                </button>
            </div>
            
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mb-6">
                <div>
                    <label for="componentName" class="block text-sm font-medium text-gray-700 mb-1">Component Name</label>
                    <input type="text" id="componentName" placeholder="e.g., Midterm Exam" class="w-full input-field px-4 py-2 border border-gray-300 rounded-lg focus:outline-none">
                </div>
                <div>
                    <label for="componentScore" class="block text-sm font-medium text-gray-700 mb-1">Your Score</label>
                    <input type="number" id="componentScore" placeholder="0-100" min="0" max="100" class="w-full input-field px-4 py-2 border border-gray-300 rounded-lg focus:outline-none">
                </div>
                <div>
                    <label for="componentWeight" class="block text-sm font-medium text-gray-700 mb-1">Weight (%)</label>
                    <input type="number" id="componentWeight" placeholder="0-100" min="0" max="100" class="w-full input-field px-4 py-2 border border-gray-300 rounded-lg focus:outline-none">
                </div>
            </div>
            
            <button id="addComponentBtn" class="w-full sm:w-auto add-btn px-6 py-3 bg-indigo-600 text-white font-medium rounded-lg hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
                <i class="fas fa-plus-circle mr-2"></i>Add Component
            </button>
        </div>
        
        <div class="bg-white rounded-xl grade-card p-6 mb-8">
            <h2 class="text-xl font-semibold text-gray-800 mb-6">Your Grade Components</h2>
            
            <div class="overflow-x-auto">
                <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-50">
                        <tr>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Component</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Score</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Weight</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Weighted Score</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="componentsTable" class="bg-white divide-y divide-gray-200">
                        <!-- Components will be added here dynamically -->
                    </tbody>
                </table>
            </div>
            
            <div id="noComponentsMsg" class="text-center py-8 text-gray-500">
                <i class="fas fa-clipboard-list text-4xl mb-3"></i>
                <p>No grade components added yet. Add your first component above.</p>
            </div>
        </div>
        
        <div id="resultContainer" class="hidden bg-white rounded-xl grade-card p-6 result-animation">
            <h2 class="text-xl font-semibold text-gray-800 mb-6">Your Final Grade</h2>
            
            <div class="flex flex-col items-center justify-center py-8">
                <div id="gradeCircle" class="w-40 h-40 rounded-full flex items-center justify-center mb-6 text-4xl font-bold">
                    <!-- Grade will be displayed here -->
                </div>
                
                <div class="text-center">
                    <p class="text-lg text-gray-600 mb-1">Your final grade is:</p>
                    <p id="finalPercentage" class="text-3xl font-bold mb-4">0%</p>
                    <p id="gradeLetter" class="text-xl font-semibold px-4 py-2 rounded-full">F</p>
                </div>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-8">
                <div class="bg-gray-50 p-4 rounded-lg">
                    <h3 class="font-medium text-gray-700 mb-3">Grade Distribution</h3>
                    <div class="h-4 bg-gray-200 rounded-full overflow-hidden">
                        <div id="gradeBar" class="h-full bg-indigo-600" style="width: 0%"></div>
                    </div>
                    <div class="flex justify-between text-xs text-gray-500 mt-2">
                        <span>0%</span>
                        <span>100%</span>
                    </div>
                </div>
                
                <div class="bg-gray-50 p-4 rounded-lg">
                    <h3 class="font-medium text-gray-700 mb-3">Grade Summary</h3>
                    <div class="space-y-2">
                        <div class="flex justify-between">
                            <span class="text-gray-600">Total Weight:</span>
                            <span id="totalWeight" class="font-medium">0%</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-gray-600">Components:</span>
                            <span id="totalComponents" class="font-medium">0</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-gray-600">Average Score:</span>
                            <span id="averageScore" class="font-medium">0%</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="mt-8 text-center text-sm text-gray-500">
            <p>Need help? Check the grade scale below:</p>
            <div class="flex flex-wrap justify-center gap-2 mt-3">
                <span class="px-3 py-1 rounded-full grade-A">A: 90-100%</span>
                <span class="px-3 py-1 rounded-full grade-B">B: 80-89%</span>
                <span class="px-3 py-1 rounded-full grade-C">C: 70-79%</span>
                <span class="px-3 py-1 rounded-full grade-D">D: 60-69%</span>
                <span class="px-3 py-1 rounded-full grade-F">F: Below 60%</span>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM Elements
            const addComponentBtn = document.getElementById('addComponentBtn');
            const resetBtn = document.getElementById('resetBtn');
            const componentsTable = document.getElementById('componentsTable');
            const noComponentsMsg = document.getElementById('noComponentsMsg');
            const resultContainer = document.getElementById('resultContainer');
            const finalPercentage = document.getElementById('finalPercentage');
            const gradeLetter = document.getElementById('gradeLetter');
            const gradeCircle = document.getElementById('gradeCircle');
            const gradeBar = document.getElementById('gradeBar');
            const totalWeight = document.getElementById('totalWeight');
            const totalComponents = document.getElementById('totalComponents');
            const averageScore = document.getElementById('averageScore');
            
            // Data storage
            let components = [];
            
            // Event Listeners
            addComponentBtn.addEventListener('click', addComponent);
            resetBtn.addEventListener('click', resetCalculator);
            
            // Functions
            function addComponent() {
                const name = document.getElementById('componentName').value.trim();
                const score = parseFloat(document.getElementById('componentScore').value);
                const weight = parseFloat(document.getElementById('componentWeight').value);
                
                // Validation
                if (!name || isNaN(score) || isNaN(weight)) {
                    showAlert('Please fill in all fields with valid values', 'error');
                    return;
                }
                
                if (score < 0 || score > 100) {
                    showAlert('Score must be between 0 and 100', 'error');
                    return;
                }
                
                if (weight < 0 || weight > 100) {
                    showAlert('Weight must be between 0 and 100', 'error');
                    return;
                }
                
                // Add component to array
                const component = {
                    id: Date.now(),
                    name,
                    score,
                    weight
                };
                
                components.push(component);
                
                // Update UI
                updateComponentsTable();
                calculateFinalGrade();
                
                // Clear input fields
                document.getElementById('componentName').value = '';
                document.getElementById('componentScore').value = '';
                document.getElementById('componentWeight').value = '';
                
                // Focus on name field for next entry
                document.getElementById('componentName').focus();
                
                showAlert('Component added successfully!', 'success');
            }
            
            function updateComponentsTable() {
                if (components.length === 0) {
                    noComponentsMsg.classList.remove('hidden');
                    componentsTable.innerHTML = '';
                    return;
                }
                
                noComponentsMsg.classList.add('hidden');
                
                // Calculate total weight for validation
                const currentTotalWeight = components.reduce((sum, comp) => sum + comp.weight, 0);
                
                // Sort components by weight (descending)
                components.sort((a, b) => b.weight - a.weight);
                
                let tableHTML = '';
                
                components.forEach(component => {
                    const weightedScore = (component.score * component.weight) / 100;
                    
                    tableHTML += `
                        <tr>
                            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${component.name}</td>
                            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${component.score.toFixed(1)}%</td>
                            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${component.weight.toFixed(1)}%</td>
                            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${weightedScore.toFixed(1)}%</td>
                            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                                <button onclick="deleteComponent(${component.id})" class="text-red-600 hover:text-red-900 mr-3">
                                    <i class="fas fa-trash-alt"></i>
                                </button>
                                <button onclick="editComponent(${component.id})" class="text-indigo-600 hover:text-indigo-900">
                                    <i class="fas fa-edit"></i>
                                </button>
                            </td>
                        </tr>
                    `;
                });
                
                componentsTable.innerHTML = tableHTML;
                
                // Show warning if total weight exceeds 100%
                if (currentTotalWeight > 100) {
                    showAlert(`Warning: Total weight is ${currentTotalWeight.toFixed(1)}% (exceeds 100%)`, 'warning');
                }
            }
            
            function calculateFinalGrade() {
                if (components.length === 0) {
                    resultContainer.classList.add('hidden');
                    return;
                }
                
                // Calculate total weighted score
                const totalWeightedScore = components.reduce((sum, comp) => {
                    return sum + (comp.score * comp.weight) / 100;
                }, 0);
                
                // Calculate total weight (might be less than 100%)
                const calculatedTotalWeight = components.reduce((sum, comp) => sum + comp.weight, 0);
                
                // Calculate final percentage (adjusted for actual total weight)
                let finalPercentageValue;
                if (calculatedTotalWeight === 0) {
                    finalPercentageValue = 0;
                } else {
                    finalPercentageValue = (totalWeightedScore / calculatedTotalWeight) * 100;
                }
                
                // Calculate average score
                const avgScore = components.reduce((sum, comp) => sum + comp.score, 0) / components.length;
                
                // Determine grade letter
                let letterGrade, gradeClass;
                if (finalPercentageValue >= 90) {
                    letterGrade = 'A';
                    gradeClass = 'grade-A';
                } else if (finalPercentageValue >= 80) {
                    letterGrade = 'B';
                    gradeClass = 'grade-B';
                } else if (finalPercentageValue >= 70) {
                    letterGrade = 'C';
                    gradeClass = 'grade-C';
                } else if (finalPercentageValue >= 60) {
                    letterGrade = 'D';
                    gradeClass = 'grade-D';
                } else {
                    letterGrade = 'F';
                    gradeClass = 'grade-F';
                }
                
                // Update UI
                finalPercentage.textContent = `${finalPercentageValue.toFixed(1)}%`;
                gradeLetter.textContent = letterGrade;
                gradeLetter.className = `text-xl font-semibold px-4 py-2 rounded-full ${gradeClass}`;
                
                // Update grade circle
                gradeCircle.innerHTML = `
                    <div class="text-center">
                        <div class="text-5xl font-bold ${gradeClass}">${letterGrade}</div>
                        <div class="text-sm mt-1 text-gray-600">${finalPercentageValue.toFixed(1)}%</div>
                    </div>
                `;
                
                // Update progress bar
                gradeBar.style.width = `${finalPercentageValue}%`;
                
                // Update summary
                totalWeight.textContent = `${calculatedTotalWeight.toFixed(1)}%`;
                totalComponents.textContent = components.length;
                averageScore.textContent = `${avgScore.toFixed(1)}%`;
                
                // Show result container
                resultContainer.classList.remove('hidden');
            }
            
            function resetCalculator() {
                components = [];
                updateComponentsTable();
                resultContainer.classList.add('hidden');
                showAlert('Calculator has been reset', 'info');
            }
            
            function deleteComponent(id) {
                components = components.filter(comp => comp.id !== id);
                updateComponentsTable();
                calculateFinalGrade();
                showAlert('Component deleted', 'info');
            }
            
            function editComponent(id) {
                const component = components.find(comp => comp.id === id);
                if (!component) return;
                
                // Fill the form with component data
                document.getElementById('componentName').value = component.name;
                document.getElementById('componentScore').value = component.score;
                document.getElementById('componentWeight').value = component.weight;
                
                // Remove the component
                deleteComponent(id);
                
                // Focus on name field
                document.getElementById('componentName').focus();
            }
            
            function showAlert(message, type) {
                // In a real app, you might use a more sophisticated alert system
                alert(`${type.toUpperCase()}: ${message}`);
            }
            
            // Make functions available globally for table buttons
            window.deleteComponent = deleteComponent;
            window.editComponent = editComponent;
        });
    </script>
</body>
</html># Grade-Calculator
