import React, { useState } from 'react';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Card, CardContent } from '@/components/ui/card';
import { Textarea } from '@/components/ui/textarea';
import { Calendar } from '@/components/ui/calendar';

export default function TodoApp() {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState({ title: '', details: '', due: null });

  const addTask = () => {
    if (newTask.title.trim() === '') return;
    setTasks([...tasks, { ...newTask, id: Date.now(), completed: false }]);
    setNewTask({ title: '', details: '', due: null });
  };

  const toggleTask = (id) => {
    setTasks(tasks.map(task => task.id === id ? { ...task, completed: !task.completed } : task));
  };

  const editTask = (id, updatedTask) => {
    setTasks(tasks.map(task => task.id === id ? { ...task, ...updatedTask } : task));
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter(task => task.id !== id));
  };

  return (
    <div className="p-6 max-w-xl mx-auto">
      <Card className="mb-4 shadow-xl">
        <CardContent className="space-y-2">
          <h1 className="text-2xl font-bold text-center">To-Do List</h1>
          <Input
            placeholder="Task Title"
            value={newTask.title}
            onChange={(e) => setNewTask({ ...newTask, title: e.target.value })}
          />
          <Textarea
            placeholder="Details (optional)"
            value={newTask.details}
            onChange={(e) => setNewTask({ ...newTask, details: e.target.value })}
          />
          <Calendar
            selected={newTask.due}
            onSelect={(date) => setNewTask({ ...newTask, due: date })}
          />
          <Button onClick={addTask}>Add Task</Button>
        </CardContent>
      </Card>

      <div className="space-y-4">
        {tasks.map(task => (
          <Card key={task.id} className="shadow">
            <CardContent className="space-y-2">
              <div className="flex justify-between items-center">
                <h2
                  className={`text-lg font-semibold ${task.completed ? 'line-through text-gray-500' : ''}`}
                >
                  {task.title}
                </h2>
                <Button onClick={() => toggleTask(task.id)}>
                  {task.completed ? 'Undo' : 'Complete'}
                </Button>
              </div>
              {task.details && <p className="text-sm text-gray-600">{task.details}</p>}
              {task.due && (
                <p className="text-sm text-gray-500">Due: {new Date(task.due).toLocaleString()}</p>
              )}
              <div className="flex gap-2">
                <Button variant="outline" onClick={() => editTask(task.id, { ...task, title: prompt('Edit title:', task.title) || task.title })}>
                  Edit
                </Button>
                <Button variant="destructive" onClick={() => deleteTask(task.id)}>
                  Delete
                </Button>
              </div>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}
